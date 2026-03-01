# Dropbear ECU First Power-Up: Current-Limited Supply Procedure

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @Joesphan, @Alice*

## Summary

When powering up a rusEFI/epicEFI Dropbear board for the first time after wiring, the recommended procedure is to use a current-limited bench power supply rather than a direct battery connection. The board should draw no more than approximately 250 mA under normal conditions; a resistor can also be used in series to limit inrush current if a bench supply is not available.

## Details

### Recommended First Power-Up Procedure

Before connecting a freshly wired Dropbear to a vehicle battery or an unconstrained supply, use a **current-limited power supply**. This protects the board from damage if there is a wiring error (short circuit, reversed polarity on a peripheral, etc.).

@Alice requested a wiring review before powering up, and @Joesphan provided the following guidance:

1. **Use a current-limited supply first.** Set the current limit on the bench supply before applying voltage.
2. **Expected current draw:** The Dropbear should not draw more than approximately **250 mA** at idle/no-load.
3. **Voltage:** At least 12 V must be provided to avoid brownouts or unexpected behavior. Ensure the supply can deliver sufficient current at that voltage.
4. **Alternative method:** If a bench supply with current limiting is not available, a **series resistor** can be placed in the supply line to limit current during the initial power-up.

### Why This Matters

A current-limited supply will simply reduce voltage (or cut off) if current exceeds the limit, rather than allowing a potential wiring fault to damage the board. Observing the actual current draw at first power-up also serves as a quick sanity check on the wiring — a draw significantly above 250 mA suggests a short or incorrect connection.

### Key Values

| Parameter | Value |
|-----------|-------|
| Expected idle current draw | ~250 mA (max) |
| Minimum supply voltage | 12 V |
| Current supply source for main relay power | 12 V from main relay |
