# ETB (Electronic Throttle Body) Feed-Forward Table in rusEFI

*Source: rusEFI Discord, 2026-02-26 | Channel: 1411925674498719775*
*Contributors: @Joesphan, @SanGawku*

## Summary

A community member is using a feed-forward table in the rusEFI ETB (Electronic Throttle Body / Drive-by-Wire) control loop and reported that it stopped working correctly after a firmware or configuration change. The discussion confirms that feed-forward is a supported and intentionally used feature in rusEFI's ETB PID implementation.

## Details

### What is Feed-Forward in ETB Control?

Feed-forward is a control technique where the controller sends a pre-computed output signal based on a known input-to-output mapping, rather than waiting for error feedback. In the ETB context:

- The **feed-forward table** maps throttle target position to a base motor drive output, reducing the work required by the PID feedback loop.
- This can improve response speed and reduce overshoot compared to pure PID control.
- The approach is analogous to what is used in flight controllers (quadcopters, etc.) where feed-forward is a standard technique for improving tracking performance.

### Known Issue (2026-02-26)

- The feed-forward table in Joesphan's rusEFI setup broke following a change (firmware update or configuration modification).
- The specific bug was not resolved in this thread — it was escalated to the firmware developer (@ggurov) for a fix.

### Configuration Notes

- ETB feed-forward is configured via the rusEFI TunerStudio interface under the ETB (Drive-by-Wire) settings.
- The feed-forward table is separate from the PID gains; both can be tuned independently.
- If the feed-forward table is broken or misconfigured, the ETB will fall back to relying solely on the PID feedback loop, which may result in sluggish or oscillatory throttle response.

### Recommendation

If you experience degraded ETB tracking after a firmware update, check whether the feed-forward table values have been reset or become invalid. Compare against a known-good tune backup and restore the table if needed.
