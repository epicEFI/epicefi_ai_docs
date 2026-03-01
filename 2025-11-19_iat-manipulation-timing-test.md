# IAT Sensor Manipulation for Timing Correction Testing
*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @fast335xi (fast335xi)*

## Summary

A technique used to test intake air temperature (IAT)-based timing correction by forcing the IAT sensor to read a very cold temperature (-32 °C) via a hardware input pull, then applying strongly negative timing corrections at that IAT value to verify the correction table is working as expected.

## Details

### Technique

- A **button or switch** is used to pull the IAT sensor signal line to a resistance or voltage that causes the ECU to read **-32 °C** (a value at the cold extreme of the IAT sensor curve).
- With the IAT now reading -32 °C, the operator sets **very negative timing corrections** in the IAT timing correction table for that temperature point.
- This allows verification that the IAT-based timing correction is actively reducing ignition advance in response to a sensor reading, confirming the table and correction logic are functioning.

### Use Case

This is a **bench or workshop test procedure**, not a permanent tuning change. It lets a tuner confirm that:
1. The IAT sensor input is being read correctly by the ECU.
2. The IAT timing correction table is being applied.
3. The correction values are having a measurable effect on ignition timing.

### Notes

- The pull to -32 °C is achieved by connecting a specific resistor value (or grounding through a calibrated resistor) that corresponds to -32 °C on the ECU's IAT sensor calibration curve.
- After testing, remove the pull resistor and restore the IAT correction table to production values.
