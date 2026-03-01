# ETB TPS Calibration: Inverted Throttle Body Fixed by Flipping H-Bridge Pins

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @hydrocarbon82, @joesphan, @sangawku, @ggurov*

## Summary

A user running stock rusEFI firmware on an electronic throttle body (ETB) setup encountered persistent inverted TPS readings: the throttle blade would go to WOT with zero pedal input and close with full pedal input. Despite multiple software-side attempts (TPS auto-cal, inverting closed/open values, swapping TPS1/TPS2 as primary), the problem persisted. The fix was physically swapping the H-bridge motor control wires (pins 1 and 2).

## Details

### Symptoms

- Auto-calibration completes but the ETB is inverted: idle position = WOT blade angle, full pedal = closed blade
- Inverting the closed/open voltage values in software makes it go even farther WOT, and causes the blade to jam shut when the pedal is fully depressed and power is cycled
- Setting TPS1 as primary: auto-cal inverts it (idle open, full pedal shut)
- Setting TPS2 as primary: calibrates but won't respond to pedal input
- TPS1 and TPS2 voltages do sweep as expected with throttle position movement

### Attempted Software Workarounds (Did Not Work)

1. Running auto-calibration and manually entering the resulting numbers
2. Inverting the ETB closed/open voltage values (swapping them in the settings)
3. Swapping TPS1 and TPS2 as primary/secondary sensors
4. Powering the ETB via direct 12V (correctly opens the blade), confirming the motor itself is fine

### Root Cause

The H-bridge motor direction wires were connected in a way that caused the firmware's "close" command to open the throttle and vice versa. This is a hardware wiring issue, not a software configuration issue. The firmware's fail-safe behavior (fail-to-close) exacerbates the symptom when values are inverted in software.

### Fix

Physically swap the two H-bridge output wires (pin 1 and pin 2) at the ETB motor connection. After the physical swap, change which TPS sensor is designated TPS1 in the software to match the corrected motor direction.

**Steps:**
1. Swap the no.1 and no.2 H-bridge motor output wires physically
2. In rusEFI/epicEFI software, reassign which sensor is TPS1 vs TPS2 to match
3. Re-run auto-calibration
4. Burn settings, disconnect USB, power-cycle the ECU

### ETB Auto-Calibration Process (General Notes)

- Disable ETB control first before reading rest position voltage
- Note the rest/limp voltage (e.g., 1.2V with ETB disabled)
- Run auto-cal to get min/max voltage values (e.g., 0.8V and 4.2V)
- Infer from rest position which end is closed vs open (rest position should be closer to the closed value)
- If the high voltage = closed and low voltage = open, swap the two values in settings
- Burn, unplug USB, then power-cycle the vehicle (important: must disconnect USB before cycling power)

### Hardware Notes

- Confirmed working ETB setups on this firmware/board combination:
  - 72mm Impala throttle body
  - 43mm Cruze throttle body
  - 87mm LS throttle body
- Board: stock rusEFI firmware on the primary ECU; a separate purple board running epicEFI firmware was also present in this setup

### Known Firmware Issue

Joesphan flagged that auto-calibration may write the same value into both the open and closed fields (a bug). This can cause TPS to appear "calibrated" but not actually track position correctly. If auto-cal values look identical, this bug may be the cause.

### Feature Request

A software-level H-bridge invert mode was discussed as a desirable addition to avoid requiring physical wire swaps for motor direction correction.
