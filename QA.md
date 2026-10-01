# Mo Training — Release QA

## P0 state regression
- [ ] Fresh Monday: Mon–Sun rail, Monday = 今天.
- [ ] Fresh Friday: Mon–Thu past, Friday = 今天, Sat–Sun future.
- [ ] Past day cannot start a workout.
- [ ] Future day cannot start a workout.
- [ ] Recovery day has no strength CTA.
- [ ] Start → running red CTA, no label flash.
- [ ] Pause → green Continue CTA, elapsed frozen.
- [ ] Resume → red Pause CTA, elapsed continues.
- [ ] Back → home shows Continue + Restart.
- [ ] Refresh home does not force L2.
- [ ] Refresh running L2 restores running state.
- [ ] Refresh paused L2 restores paused state.
- [ ] Tab switch pauses/persists session without completing it.
- [x] Early end = ended_early, never completed. *(static state-path verified)*
- [x] Full completion = completed. *(static state-path verified)*
- [ ] Completion state agrees across all three tabs.

## Time/history regression
- [x] Preserve legacy 2026-10-03 completion. *(static state-path verified)*
- [ ] 2026-10-04 with no session displays 未训练.
- [ ] 2026-10-05 is a fresh Monday session.
- [x] History previous week supports 9/28–10/4 navigation. *(static state-path verified)*
- [x] History current week resolves 10/5–10/11. *(static state-path verified)*
- [x] Future history navigation disabled. *(static state-path verified)*
- [ ] Calendar October reflects 10/3 complete and 10/4 not trained.
- [ ] Month boundary and year boundary.
- [ ] Local date identity unaffected by UTC conversion.

## Workout logging
- [ ] Weight/reps/RIR persist per set.
- [ ] Set completion updates progress and volume.
- [ ] All sets complete changes secondary action to 完成训练.
- [ ] 90s rest starts after set completion.
- [x] Rest skip restores session CTA ownership in code path. *(static verified; device QA pending)*
- [ ] Exercise-level complete toggles intended sets only.
- [ ] Restart clears only selected session data after destructive confirmation.

## UI / device
- [ ] iPhone Safari portrait.
- [ ] Installed PWA portrait.
- [ ] Safe-area top/bottom.
- [ ] Long workout scroll.
- [ ] Keyboard does not hide active set inputs/actions.
- [ ] L1→L2 push has no first-frame flash.
- [ ] Reduced Motion.
- [ ] 320–430px widths.
- [ ] No horizontal page overflow.
- [ ] Tap targets >=44px.

## PWA / release
- [x] Service worker deletes old caches on activation and claims clients. *(code verified)*
- [x] Navigation uses network-first strategy when online. *(code verified)*
- [x] Navigation falls back to cached index offline. *(code verified)*
- [ ] No console errors.
- [ ] No duplicate intervals after repeated pause/resume.
- [ ] No P0/P1 blockers before release.
