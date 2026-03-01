# Fuel Trim Behavior: Short-Term and Long-Term Interaction
*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov (ggurov)*

## Summary

Explains the interaction between short-term fuel trim (STFT) and long-term fuel trim (LTFT) in rusEFI/epicEFI. If STFT is not actively learning, LTFT continues applying its previously learned corrections without incorporating any new STFT adjustments.

## Details

### STFT and LTFT Relationship

- **Short-term fuel trim (STFT)**: Makes immediate, transient corrections to fueling based on real-time feedback (typically O2/lambda sensor).
- **Long-term fuel trim (LTFT)**: Accumulates STFT corrections over time to build a persistent fuel correction map.

### Behavior When STFT Is Not Learning

If STFT is not adapting (e.g., closed-loop fueling is disabled, the sensor is out of range, or learning conditions are not met):
- LTFT will continue to apply the corrections it previously learned.
- LTFT will **not** receive any new updates from STFT.
- The system is effectively frozen — running on whatever LTFT already contains.

### Diagnostic Implication

If a fueling problem is suspected and STFT appears not to be learning, check:
- Whether closed-loop fueling is enabled
- Whether the lambda/O2 sensor is in its operating range and reporting valid data
- Whether operating conditions (RPM, load, coolant temp) meet the learning window criteria

LTFT running on stale values while STFT is inactive can mask an underlying calibration issue.
