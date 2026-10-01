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
- [ ] Early end = ended_early, never completed.
- [ ] Full completion = completed.
- [ ] Completion state agrees across all three tabs.

## Time/history regression
- [ ] Preserve legacy 2026-10-03 completion.
- [ ] 2026-10-04 with no session displays 未训练.
- [ ] 2026-10-05 is a fresh Monday session.
- [ ] History previous week shows 9/28–10/4.
- [ ] History current week shows 10/5–10/11.
- [ ] Future history navigation disabled.
- [ ] Calendar October reflects 10/3 complete and 10/4 not trained.
- [ ] Month boundary and year boundary.
- [ ] Local date identity unaffected by UTC conversion.

## Workout logging
- [ ] Weight/reps/RIR persist per set.
- [ ] Set completion updates progress and volume.
- [ ] All sets complete changes secondary action to 完成训练.
- [ ] 90s rest starts after set completion.
- [ ] Rest skip restores correct Pause/Continue CTA.
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
- [ ] Existing v1 cache upgrades to current cache.
- [ ] Navigation receives newest index when online.
- [ ] Offline launch falls back to cached shell.
- [ ] No console errors.
- [ ] No duplicate intervals after repeated pause/resume.
- [ ] No P0/P1 blockers before release.
