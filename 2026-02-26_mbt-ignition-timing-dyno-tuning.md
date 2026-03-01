# Finding MBT Ignition Timing on a Dyno (Steady-State Method)

*Source: epicEFI Discord, 2026-02-26 | Channel: 1411925674498719775*
*Contributors: @Joshan_Lu*

## Summary

Maximum Brake Torque (MBT) timing is the ignition advance point at which the engine produces its highest torque output. This guide describes a steady-state dyno method for locating MBT by advancing timing in 1-degree increments while monitoring torque and listening for knock. The procedure requires holding RPM and boost constant throughout.

## Details

### Test Setup

- **Dyno mode:** Steady-state (not sweep)
- **Test RPM:** ~4000 RPM
- **Boost:** Hold constant throughout the test — do not allow boost pressure to vary between timing steps, as this would confound the torque measurement

### Procedure

1. Establish a stable steady-state run at 4000 RPM with constant boost.
2. Note the current torque output.
3. **Advance ignition timing by 1 degree.**
4. Observe torque:
   - **Torque increases:** The engine is still below MBT. Continue advancing.
   - **Torque does not increase:** MBT has been reached (or exceeded). Back off 1 degree to the previous timing value.
5. Repeat across the desired load/RPM cells.

### Knock Detection

- **Listen for knock (detonation) throughout the process.** The engine may begin to detonate before MBT is reached, particularly under boost.
- If knock is detected at any timing step, immediately retard timing back to the last clean value. Do not continue advancing.
- MBT is only valid as long as the engine runs cleanly — knock-limited timing may be lower than true MBT on boosted applications.

### Identifying MBT

- MBT is confirmed when advancing timing by 1 degree **does not produce a measurable torque increase**.
- The timing value just before this plateau is the MBT point for that operating condition.
- Record and table the MBT timing for each RPM/load combination tested.

### Important Caveats

- This procedure must be repeated at each RPM and load cell of interest — MBT varies with engine speed and load.
- On boosted engines, knock-limited timing may prevent reaching true MBT at high loads. Use knock detection (both audible and sensor-based) as a safety limit.
- Constant boost is critical: any boost variation between steps will make the torque comparison invalid.
