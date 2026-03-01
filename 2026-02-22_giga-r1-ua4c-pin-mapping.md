# Arduino Mega Giga R1 with UA4C: Pin Mapping and PA7 Fuel Output 4 Issue

*Source: epicEFI Discord, 2026-02-22 | Channel: 1401895238481744052*
*Contributors: @MykkH (@mykkh), @ggurov (@ggurov)*

## Summary

User MykkH confirmed that the Arduino Mega Giga R1 board is functional with the UA4C epicEFI/rusEFI firmware build. One issue was identified: Fuel Output 4 is assigned to pin PA7 in the Arduino Mega mapping, but PA7 does not appear in the epicEFI firmware pin dropdown (which starts at PA8). This is because PA7 is an analog pin on the STM32, and the fuel output dropdown only lists digital-capable pins. The Arduino Mega mapping of D5 to PA7 (an analog GPIO) was flagged as a questionable design choice.

## Details

### Hardware Compatibility

- **Board:** Arduino Mega Giga R1
- **Firmware variant:** UA4C (epicEFI/rusEFI)
- **Status:** Working — all digital and analog inputs confirmed functional

### Pin Mapping Issue: Fuel Output 4 / PA7

The Arduino Mega assigns Fuel Output 4 to digital pin **D5**, which maps to STM32 pin **PA7** on the Giga R1.

| Arduino Mega Label | Arduino Pin | STM32 Pin |
|--------------------|-------------|-----------|
| Fuel Output 4      | D5          | PA7       |

**Problem:** The epicEFI firmware pin dropdown for fuel outputs begins at PA8 and does not include PA7. This means Fuel Output 4 cannot be selected from the dropdown when using a Giga R1 with the standard UA4C firmware.

**Root cause:** PA7 is an analog GPIO on the STM32. It is excluded from the digital output selection list in the firmware. The Arduino Mega board definition's assignment of D5 = PA7 (an analog pin) for a fuel injector output is considered a non-standard mapping.

### Workaround / Context

- At the time of the discussion, only one user (MykkH) was running epicEFI on a Giga R1, and they had the board available opportunistically (not as a target deployment platform)
- ggurov noted a custom build variant file (`mega144.yaml`) was available that lists all 144 LQFP pins, which can be edited to customize pin names and dropdown labels for specific hardware configurations
- Custom pinout files allow replacing raw pin labels (e.g., "PD7") with meaningful names (e.g., "IGN3") in the TunerStudio dropdown — useful if the same hardware layout is consistent across multiple installations

### Custom Pinout Configuration (General)

For any board variant with custom pinouts:

1. Obtain the base `mega144.yaml` (or equivalent) variant file from ggurov/epicEFI
2. Edit the file to map STM32 port/pin names to your specific wiring labels
3. Build a custom firmware variant with this file
4. Dropdowns in TunerStudio will then show human-readable names instead of raw pin labels (e.g., "FUEL_4" instead of "PA7")

This is a build-time customization and requires consistency across all installations using the same variant.
