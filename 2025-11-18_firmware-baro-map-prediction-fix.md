# Firmware Update: Barometric Pressure and MAP Prediction Fix

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @ggurov*

## Summary

A new firmware build was released on 2025-11-18 addressing issues with barometric pressure (baro) correction and MAP (Manifold Absolute Pressure) prediction. Users who were experiencing problems with these calculations should update to the new firmware.

## Details

### Release Note

> "new firmware is up, %baro + map prediction should be fine" — @ggurov

The update resolves problems with:
- **%baro correction**: Percentage-based barometric pressure compensation used in fuel and ignition calculations.
- **MAP prediction**: The algorithm used to predict manifold pressure (relevant to transient fueling, load calculation, and boost control).

### Action Required

Users experiencing erratic load calculations, fueling errors at altitude, or incorrect boost/MAP readings should update to this firmware build. No specific version number was provided in this message; check the rusEFI/epicEFI firmware release channel for the build corresponding to 2025-11-18.
