# Firmware 2.20: Cranking Ignition Fix

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @ggurov, @MykkH*

## Summary

Firmware version 2.20 was released on 2026-02-20 and includes a fix for a cranking ignition timing issue. The fix may cause the ignition event to occur one engine cycle earlier than in previous firmware, which is a known behavioral change rather than a new bug.

## Details

### Release

- @ggurov announced: "new firmware up 2.20"
- Later confirmed: "yes, 2.20 has been updated to have the ignition fix"

### Cranking Ignition Behavior Change

@MykkH observed after updating to 2.20:

> "Is current 2-20 with the cranking ignition fix? I think it is starting a cycle earlier now."

This indicates that the ignition fix adjusted the timing of the first spark event during cranking, causing it to fire approximately **one engine cycle earlier** than the previous behavior. This is consistent with a correction to when the ECU schedules the first ignition event relative to crank sync.

### Impact

- Users upgrading from pre-2.20 firmware should be aware that cranking ignition timing behavior has changed.
- If cranking felt correct before 2.20, review the cranking ignition advance/timing settings after updating — the "earlier cycle" shift may require adjustment.
- The fix addresses a pre-existing issue where the ignition event was being scheduled too late during the cranking sequence.

### Related Context

This fix is related to the broader discussion about PRE_SYNC fuel delivery and the "no nada" revolution observed during cranking (see thread on cranking injection modes). The firmware 2.20 update was specifically aimed at correcting the ignition scheduling path.
