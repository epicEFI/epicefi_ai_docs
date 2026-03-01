# Fuel Pressure Input Units: kPa Required, Not PSI

*Source: rusEFI Discord, 2026-02-22 | Channel: 1356732771325968630*
*Contributors: @Robb235, @ggurov*

## Summary

A user entered fuel pressure in PSI in the epicEFI configuration UI, not realising the field expects kPa. This caused the ECU to run extremely rich because it calculated fuel pressure as only ~44 kPa (~6.4 PSI) instead of the intended value. The fix (entering the correct kPa value) resolved fuelling. A UI improvement was requested and acknowledged by the developer.

## Details

### The Problem

The fuel pressure configuration menu in epicEFI accepts values in **kPa**, but the field has no unit label. A user typed a PSI value (e.g., 44 PSI ≈ 303 kPa) directly into the field, which the ECU interpreted as 44 kPa — far below any real fuel pressure.

Result: the engine ran "reaaaaaaallly rich" because the ECU calculated injector pulse width based on far too little fuel pressure.

### Fix

Enter fuel pressure in **kPa**. Common conversions:
- 44 PSI ≈ 303 kPa
- 58 PSI ≈ 400 kPa (typical returnless/high-pressure system)
- 43.5 PSI = 300 kPa

### Returnless Fuel System Configuration

For a standard returnless (deadhead) fuel system with a fixed fuel pressure regulator:
1. Select **"Returnless"** as the fuel system type in the settings
2. Set the fixed fuel pressure in **kPa**

### Developer Action

ggurov confirmed that unit labels will be added to the fuel pressure input field to prevent this confusion in future firmware versions.
