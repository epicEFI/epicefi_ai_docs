# Trigger Wheel Synchronization with Non-Uniform Tooth Spacing (Time-Domain)

*Source: rusEFI Discord, 2025-11-27 | Channel: 1411925674498719775*
*Contributors: ggurov (@ggurov), Joesphan (@joesphan), Joshan Lu (@joshanlu), Robb235 (@robb235)*

## Summary

Time-domain trigger wheel synchronization can fail when the gap tooth is not sufficiently larger than adjacent normal teeth, particularly when engine compression causes tooth-interval distortion that mimics or obscures the sync gap. The default "following tooth" bounds in rusEFI may be out of range for wheels with unusual tooth geometry, requiring manual tuning of the gap ratio window. An additional practical complication is accurately counting teeth on physically worn or modified wheels.

## Details

### The Problem: Compression Waves Interfering with Gap Detection

rusEFI's time-domain synchronization identifies the sync gap by comparing the current tooth interval to the preceding tooth interval. The expected ratio for missing-tooth wheels (e.g., 36-2) is approximately 1.9–2.3 for a 2-missing-tooth gap, versus a ratio near 1.0 for consecutive normal teeth.

On a specific motorcycle engine with a non-standard trigger wheel, the following was observed:

- The gap tooth interval was **50 ms**; the following tooth was **27 ms** — a ratio of ~1.85
- Under cranking with significant compression pulses, the crankshaft speed variation mimics a "slow" tooth, creating false gaps or preventing detection of the real gap
- Setting the wheel as **24-2** (originally) failed to sync reliably because the compression slow-down was large enough to fall within the gap-detection window

### Workaround: Increasing the Gap Size

To make the sync gap unambiguous relative to compression-induced variation, a tooth was physically removed from the trigger wheel, changing the configuration:

- Original: **24-2** (24 teeth total, 2 missing)
- Modified: **23-3** (one additional tooth removed, making a 3-missing-tooth gap)

This produced a larger ratio between the gap interval and the adjacent tooth interval, reducing false-sync events caused by compression waves. The engine synced correctly as 24-2 without compression (no compression waves to cause distortion) but required the modified 23-3 wheel to sync reliably under real compression.

### Cam Sync as Alternative

If physically modifying the trigger wheel is not acceptable, adding a cam sync sensor was discussed as an alternative. A cam channel can provide unambiguous cycle reference independently of crank tooth interval variation, bypassing the gap-detection problem entirely.

### Default "Following Tooth" Bounds

ggurov noted that the firmware defaults for the "following tooth" comparison range may be out of bounds for certain wheels. If a trigger wheel has an unusually-shaped gap or the ratio between the gap tooth and the following tooth falls outside the default window, sync will fail even when the wheel geometry is correct. In such cases, the tooth ratio bounds need to be adjusted in firmware configuration (see `setTriggerSynchronizationGap3`).

### Tooth Counting Caution

During debugging, the actual tooth count on the physical wheel was disputed:

- The wheel was originally assumed to be 24-2
- Physical inspection and counting during the session revealed discrepancies (counts of 20, 21, and 24 were all observed by different participants examining oscilloscope captures and photos)
- Recommendation: count teeth directly from a trigger scope capture in TunerStudio or the rusEFI console rather than relying on part number or visual inspection of photos

### Tooth Spacing Note

On this wheel the teeth were **not evenly spaced** — the gap position could be moved by flipping the trigger wheel around on the shaft. This adds another variable: the gap timing relative to TDC affects how close the sync event falls to the compression slowdown zone. Placing the gap at the slowest part of the compression cycle (near TDC compression) risks the compression slow-down being mistaken for the gap or post-gap transition; placing it at the fastest part of the cycle (away from TDC) provides the clearest discrimination.

As noted by Joshan Lu: timing the gap to occur at the slowest part of the compression cycle is risky — if the engine is slow there, the post-gap tooth can look deceptively similar to the gap itself.
