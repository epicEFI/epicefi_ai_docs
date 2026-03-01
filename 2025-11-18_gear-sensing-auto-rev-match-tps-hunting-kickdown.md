# Gear Sensing, Auto Rev Match, TPS Hunting, and Kickdown Behavior

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @joesphan, @tq2o2ese994zh1bswdm8aj9paym9rxzu (Psychedelia), @ggurov, @sev.1337*

## Summary

Several related threads on 2025-11-18 discussed gear-related functionality in rusEFI/epicEFI: a user's third gear mechanical failure (stripped synchronizer or gear), interest in enabling gear sensing and auto rev-match features, observed TPS hunting behavior, kickdown timing (3 seconds for full downshift process), and a report of unintended downshifts from slight throttle blips on a BMW X3M.

## Details

### Third Gear Mechanical Failure

A user reported that their third gear had degraded from being "sparkly" (noisy but engaging) to not engaging at all:

> "Oh mine was sparkaly before but now 3rd doesn't even try to mesh, either my syncro is gone or the whole 3rd gear is done but someting wong" — @Psychedelia

This indicates either a failed synchronizer or complete third gear damage. This is a mechanical issue independent of the EFI system, but was raised in context of attempting to enable gear sensing functionality.

### Gear Sensing and Auto Rev Match Interest

Auto rev-match and gear position sensing were identified as desirable features to implement:

> "Really want to try to get gear sensing and auto rev match going" — @joesphan

Gear sensing in rusEFI/epicEFI allows the ECU to know the current gear position, enabling features such as:
- Gear-specific fueling or ignition maps
- Automatic throttle blip on downshift (auto rev-match)
- Gear-based speed/RPM limiters

### TPS Hunting

A user (@SeV) reported throttle position sensor hunting behavior visible in logs:

> "Look at the main throttle position hunting" — @sev.1337

TPS hunting (oscillating TPS reading) can result from electrical noise, improper grounding (see separate grounding doc), or tuning issues with the throttle control system. The issue was noted alongside kickdown behavior, suggesting it may be related to transmission control interactions.

### Kickdown Timing

The full kickdown (downshift) process was observed to take approximately 3 seconds:

> "and here's the kickdown, the whole process of downshifting takes 3 seconds" — @sev.1337

This 3-second duration for the complete downshift sequence (from kickdown trigger to completed gear change and returned throttle) is a reference point for evaluating transmission control responsiveness.

### Unintended Downshift from Throttle Blip (BMW X3M)

On a BMW X3M, a slight throttle blip was enough to trigger a downshift that then held the lower gear:

> "in the x3m, if i slightly blip the throttle, it downshifts, and holds it there for a few" — @ggurov

This suggests the kickdown sensitivity threshold may need adjustment in the transmission control strategy — minor throttle inputs should not trigger a sustained downshift event. The shift logic should distinguish between a brief blip and a sustained throttle increase requesting a lower gear.
