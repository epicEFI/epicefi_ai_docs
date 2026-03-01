# Trigger Synchronization Gap Configuration in rusEFI/epicEFI

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov)*

## Summary

Example trigger synchronization gap settings for a three-channel trigger decoder configuration in rusEFI/epicEFI firmware. These values define the acceptable ratio range for gap detection used during engine synchronization.

## Details

The `setTriggerSynchronizationGap3(channel, min_ratio, max_ratio)` function configures the expected tooth-gap ratio window on each trigger wheel channel. The ECU uses these ratios to identify the sync tooth (the missing tooth or special gap) that marks a known crank/cam position.

**Example configuration:**

```cpp
s->setTriggerSynchronizationGap3(0, 0.9, 1.8);
s->setTriggerSynchronizationGap3(1, 0.9, 1.8);
s->setTriggerSynchronizationGap3(2, 0.3, 0.8);
```

- **Channel 0:** Gap ratio window 0.9–1.8 (wide tolerance, suitable for channels where the gap is approximately equal to or up to ~2x a normal tooth gap)
- **Channel 1:** Gap ratio window 0.9–1.8 (same as channel 0)
- **Channel 2:** Gap ratio window 0.3–0.8 (tighter, narrower gap — e.g., a cam sync tooth that produces a shorter-than-normal interval)

**Usage context:**
- These settings are written in C++ firmware configuration code (engine-specific decoder file).
- The ratio is computed as `(current_gap / previous_gap)`. A value of 1.0 means equal spacing; values below 1.0 indicate a shorter gap, values above 1.0 indicate a longer gap.
- Channel 2's narrow window (0.3–0.8) is typical for a cam sensor producing a single short sync pulse per engine cycle.

**Notes:**
- Setting the window too narrow can cause sync loss on noisy signals; too wide can cause false sync detection.
- Adjust these values iteratively while monitoring the trigger scope in TunerStudio or rusEFI console.
