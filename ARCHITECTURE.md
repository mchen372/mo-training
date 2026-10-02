# Mo Training — Product & State Architecture

## Product contract
Mo Training is a weekly hypertrophy training companion. The UI has three primary jobs:
1. **Mo Training** — what is my plan this natural week, and what do I do today?
2. **Calendar** — on which dates did I train?
3. **History** — what exactly happened in previous weeks/sessions?

## Time model
- A week is always Monday → Sunday.
- A plan is weekday-based and reusable.
- A workout is date-based. Date identity is local `YYYY-MM-DD`; never UTC ISO slicing.
- Past dates are view-only, today is actionable, future dates are preview-only.
- History may navigate backward by natural week; future history is disabled.

## Session state machine
```
planned
  └─ start → in_progress
               ├─ pause → paused ─ resume → in_progress
               ├─ finish all sets → completed
               └─ end early → ended_early
rest (non-strength day)
missed (past planned day without a completed/ended session)
```

Only `completed` counts as completed. `ended_early` retains partial data but is never promoted to completed.

## Source of truth
All primary surfaces must derive status from the same date-level resolver:
- Mo Training
- Calendar
- History

Legacy week+weekday keys are read for backward compatibility. New status data is mirrored to date-based keys. This compatibility layer can be removed only after an explicit migration release.

## Navigation vs session
Navigation is not session state.
- Leaving the workout page pauses and persists elapsed time.
- Switching tabs must never delete or complete a session.
- Returning to Mo Training exposes Continue / Restart when a session exists.
- Refresh/relaunch must preserve session and route semantics.

## CTA ownership
Primary CTA has one owner: workout session renderer.
- Running: red “暂停训练 + elapsed”
- Paused: green “继续训练 + elapsed”
- Rest countdown must not permanently overwrite session CTA.
Secondary CTA owns end/finish semantics.
Progress/stat renderers may never mutate primary CTA copy.

## Release principles
- No duplicated status calculations.
- No hidden destructive transitions.
- No UI state that cannot be reconstructed from persisted state.
- No production claim without target-device validation.


## Canonical session record v2

New writes use a versioned, date-bound record:

`mo-session-v2-YYYY-MM-DD`

Shape:
- `schema: 2`
- `date`
- `status`
- `startedAt`
- `elapsedMs`
- `calories`
- `endedAt` when terminal
- `updatedAt`

`readSession(date)` provides non-destructive migration by falling back to the existing date status/duration/calorie keys when a v2 record does not yet exist. `writeSession(date, patch)` persists the canonical record and temporarily mirrors legacy date keys for backwards compatibility. Existing set-level history is not rewritten or deleted.

Migration rule: legacy data remains readable; new session lifecycle transitions write v2. Once production/device QA proves migration stability, legacy session mirrors can be retired in a later schema release.


## State transition guard

Session lifecycle mutations must go through `transitionSession(date, next, patch)`. Legal transitions are explicitly enumerated:
- planned → in_progress
- missed → in_progress
- in_progress → paused / completed / ended_early
- paused → in_progress / completed / ended_early
- completed → in_progress (explicit restart only)
- ended_early → in_progress (explicit restart only)
- rest → no training transition

UI event handlers should not directly invent session status values.

## Set storage boundary

Set-level reads/writes now go through `SetStore`:
- `read(date, exercise, set)`
- `write(date, exercise, set, patch)`
- `clearDay(date)`

The current implementation intentionally mirrors the existing `mo3-...` keys so existing training history remains intact. This creates a storage boundary that can later migrate sets into the canonical session document without changing UI/business logic.
