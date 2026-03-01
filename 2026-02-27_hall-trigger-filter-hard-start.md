# Hall Trigger Filter Trade-off: Hard Start vs. Idle Trigger Errors (36-2 / 3-Tooth Cam)

*Source: epicEFI Discord, 2026-02-27 | Channel: 1401895238481744052*
*Contributors: @Aboy, @ggurov*

## Summary

A user running a Hall-effect trigger setup with a 36-2 crank wheel and a 3-tooth cam sensor discovered a trade-off between enabling and disabling the trigger filter. With the filter enabled, the engine was hard to start but produced zero trigger errors at idle. With the filter disabled, the engine started easily but generated random trigger errors at idle. The developer suggested adjusting the trigger gap parameter as a potential way to resolve the hard-start condition while keeping the filter on.

## Details

### Observed Symptom

- Trigger setup: **Hall effect, 36-2 crank / 3-tooth cam**
- **Filter ENABLED**: Engine is hard to start. Zero trigger errors at idle.
- **Filter DISABLED**: Engine starts easily. Random trigger errors appear at idle.
- User tried changing capacitor values (various capacitor substitutions) — no change to the condition.

### Workaround / Suggestion

- Developer (@ggurov) suggested exploring the **trigger gap** setting as a tuning knob to make starting easier while keeping the filter enabled.
  - Rationale: adjusting the gap threshold may allow the filter to pass the slower/weaker signal during cranking without requiring the filter to be turned off entirely.
- Recommended keeping the trigger filter **on** and tuning around it rather than disabling it.

### Diagnostic Approach

When an engine is hard to start, the recommended diagnostic is standard **data logging during cranking**:
- Log normally while cranking and during hard-start conditions.
- Key channels to watch: RPM signal quality, trigger errors, fuel pressure, lambda.
- Use the **trigger logger** (accessible in the right-click menu under "Sync") to observe trigger pattern in real time.
- In the trigger logger: a **red line** jumping high indicates a trigger error event.

### Related Notes

- The trigger filter is designed to suppress noise on the trigger signal; disabling it can let false edges through at idle (hence the random errors).
- The hard-start with filter on is likely due to the filter rejecting the weak or slow signal edges produced during low-cranking-speed rotation.
- Suggested next step: capture a data log while cranking to confirm whether the trigger is being read at all or whether it is being filtered out entirely.
