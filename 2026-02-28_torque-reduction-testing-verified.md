# Torque Reduction Functionality — Axis Fix and Stim Verification

*Source: epicEFI Discord, 2026-02-28 | Channel: 1401895238481744052*
*Contributors: @ggurov, @tmbryhn*

## Summary

A bug affecting axis configuration in the torque reduction feature was identified and fixed by @ggurov. All torque reduction functions were subsequently tested on a stimulator (stim) by @tmbryhn and confirmed working.

## Details

### Bug Fixed

An axis issue existed in the torque reduction table/configuration. This was corrected in the firmware as of 2026-02-28. No specific axis (RPM, load, etc.) was named in the conversation, but the fix was necessary for torque reduction to operate correctly.

### Stim Verification

@tmbryhn ran the full set of torque reduction functions against a hardware stimulator:

- All torque reduction functions passed testing on the stim.
- No anomalies or remaining issues were reported after the axis fix was applied.

### Context: Why Torque Reduction Matters

Torque reduction is used in transmission-integrated setups where the TCU requests the ECU to momentarily reduce engine torque during gear changes (common in automatic and DCT applications). Correct axis configuration is required for the reduction map to be applied at the right operating points. Smooth, consistent fueling is a prerequisite — jerky fuel delivery will propagate directly to jerky torque output, which can cause rough shifts even if the torque reduction logic is otherwise correct.

## Notes

- Stim testing validates the control logic path but not real-world torque accuracy, which depends on injector characterization and load cell / transducer inputs if torque is displayed on the dashboard.
- The axis fix is part of the main firmware; users running older builds should update to get the corrected torque reduction behavior.
