# Fuel Trim Instability: Wall Wetting and Map Multiply Interaction

*Source: rusEFI Discord, 2026-02-21 | Channel: 1401895238481744052*
*Contributors: @ggurov*

## Summary

When experiencing unexpectedly minor or unstable fuel adjustments during tuning, two common culprits in rusEFI/epicEFI are wall wetting compensation being enabled and the "map multiply" feature being active. Disabling map multiply and allowing sufficient settling time when wall wetting is on can resolve unexpected fueling fluctuations.

## Details

### Symptoms

A user was experiencing "minor adjustment" issues — fueling corrections that appeared inconsistent or not settling properly during tuning.

### Diagnosis and Fix

@ggurov identified two potential causes:

1. **Wall Wetting Enabled**: If the wall wetting (fuel film) compensation is active, the ECU applies transient fuel corrections for throttle changes. During a static tuning session, these corrections can cause the measured fueling to appear unstable if insufficient time is allowed for conditions to settle after each change.
   - Fix: Wait longer between changes to allow wall wetting corrections to settle, or temporarily disable wall wetting during static VE table tuning.

2. **Map Multiply Active**: The "map multiply" feature scales fueling based on MAP sensor input. If enabled during tuning, it can interact with other corrections and cause unexpected fueling adjustments.
   - Fix: **Disable map multiply** during tuning sessions to isolate base fueling behavior.

### Key Takeaways

- If fueling feels unstable or corrections seem off during tuning, check whether **wall wetting** is enabled and allow adequate settling time.
- **Disable "map multiply"** to remove an additional fueling modifier that can complicate diagnosis.
- These are standard rusEFI/epicEFI settings accessible in the fuel configuration section.
