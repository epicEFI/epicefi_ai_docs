# ADC and MAP Sensor Voltage Limits: mega128 / H7 Green Board

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @Ognjen Galic, @kitaji_SR20*

## Summary

The ADC (analog-to-digital converter) on the mega128/mega144-based H7 Green board (as used in Speeduino 0.44d) has a maximum readable voltage of approximately 4.7 V. Since almost all MAP sensors output a maximum of 4.5 V, this is a tight margin and must be accounted for when selecting and wiring sensors.

## Details

### H7 Green Board ADC Limit

- **Hardware:** H7 Green board, as featured in **Speeduino version 0.44d**, using a mega128/mega144 microcontroller.
- **Maximum ADC input voltage:** approximately **4.7 V**.
- Note: one message in the thread stated "0.47V" which appears to be a typo for 4.7 V based on context.

### MAP Sensor Voltage

- Nearly all MAP sensors have a **maximum output of 4.5 V** at full-scale pressure.
- This leaves only ~0.2 V of headroom before the ADC clips on the H7 Green board.
- Sensors with higher full-scale outputs (e.g., 5 V) will be clipped and read incorrectly.

### Recommendations

- Verify that any MAP sensor used outputs no more than 4.5 V at maximum expected manifold pressure.
- When selecting a MAP sensor for use with a mega128/mega144-based ECU, confirm the output voltage range is within the ADC's 4.7 V limit.
- If using a higher-output sensor, a voltage divider may be necessary to scale the signal into range.
