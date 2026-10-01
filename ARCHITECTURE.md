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
