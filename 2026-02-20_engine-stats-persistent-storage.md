# Persistent Engine Statistics Storage on rusEFI/epicEFI

*Source: rusEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @ggurov*

## Summary

@ggurov raised interest in adding persistent engine statistics tracking to rusEFI/epicEFI, leveraging the SD card as a non-volatile storage medium for key performance metrics. This is a proposed/in-progress capability, not necessarily a shipped feature as of this date.

## Details

### Proposed Statistics to Track

The following engine statistics were identified as candidates for persistent storage (saved to SD card so they survive power cycles):

- **Max PSI** (peak boost pressure recorded)
- **Max RPM** (peak engine speed recorded)
- **Rev limiter activation count** (how many times the rev limiter has triggered)
- **Miles driven** (if odometer/distance data is accessible from the ECU's sensor data)

### Storage Mechanism

With SD card logging enabled, the ECU can write data while the engine is running. Persistent stats would leverage this same write pathway, allowing statistics to survive ignition cycles and power losses.

### Status

This was expressed as a personal interest/TODO by @ggurov ("i wanna keep some stats that can be saved") and not confirmed as an implemented feature. The feasibility of "miles driven" tracking depends on whether vehicle speed sensor data is available and integrated in the firmware's runtime.
