# Trigger Edge Settings: Cam Rising, Crank Falling, and ECU Edge Interpretation

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: Joesphan (@joesphan), ggurov (@ggurov)*

## Summary

Notes on configuring trigger edge polarity for cam and crank sensors in rusEFI/epicEFI, and how the ECU reports which edge it is interpreting as the trigger event.

## Details

**Edge polarity per sensor:**
- **Cam sensor:** edges set to **rising** (the ECU triggers on the rising edge of the cam signal).
- **Crank sensor:** trigger specified as **falling** (the ECU triggers on the falling edge of the crank signal).

This is a common setup when the cam and crank sensors produce inverted signal shapes relative to each other, or when the physical tooth/wheel geometry means the relevant timing edge is different for each sensor.

**ECU edge interpretation readout:**
The rusEFI ECU reports which trigger edge it is currently interpreting as a valid event. This readout ("it tells what trigger the ECU is interpreting as an edge") is useful during commissioning to verify that:

1. The ECU is receiving a signal at all.
2. The ECU is locking on to the correct edge (rising vs. falling).
3. Misconfiguration (e.g., both sensors set to the same edge when one should be inverted) can be caught by observing whether the decoded trigger pattern matches expectations.

**Troubleshooting tip:**
If the engine fails to sync, confirm:
- That cam edges are set to rising and crank to falling (or vice versa, depending on the specific sensor output waveform).
- Use the rusEFI trigger scope (TunerStudio or rusEFI console) to visualize the raw signal and confirm edge polarity before adjusting settings.
