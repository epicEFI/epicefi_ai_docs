# Ground-Triggered Inputs on Mega Board: Pull-Up Resistors and Ground Safety

*Source: rusEFI Discord, 2025-11-24 | Channel: hardware-help (1411925674498719775)*
*Contributors: GGGI (@.gunadi), Walter R. (@walterronny), Joesphan (@joesphan)*

## Summary

Brief but useful clarification on the electrical behavior of ground-triggered digital inputs on the rusEFI Mega board, the importance of pull-up resistors to prevent floating inputs, and why a ground reference does not carry or transmit static electricity.

## Details

### Ground-Triggered Inputs

The rusEFI Mega board uses ground-triggered digital inputs: the input pin is active when pulled to ground (0 V) and inactive when floating or pulled high. This is the standard automotive low-side switching convention.

### Risk of Floating Inputs

If a ground-triggered input is left unconnected (floating), the pin has no defined state and will read random noise, causing false triggers or erratic behavior.

**Recommendation (Walter R.):** Add a **pull-up resistor** between the input pin and the signal voltage rail (typically 5 V or 12 V depending on the input type). This ensures the pin reads a defined high state when the switch is open, and a clean low when the switch closes to ground.

Additionally, reverse polarity protection can be added on the input line to prevent damage if the signal is accidentally connected to voltage instead of ground.

### Static Electricity and Ground Lines

GGGI asked whether a ground connection could carry or introduce static electricity to the board.

Joesphan's answer: *"Ground is ground. Reference 0 V. Everything is something, but references are defined."*

- A ground line is simply a reference potential (0 V relative to the circuit). It does not carry static charge in normal automotive wiring practice.
- Static electricity risks arise from ungrounded surfaces discharging into circuit pins during handling, not from a properly bonded ground connection.
- Voltage is always a **potential difference** between two nodes. If the Mega is powered with 5 V across its VIN and GND pins, it operates correctly regardless of the absolute voltage level of those nodes relative to any external reference (e.g., 120 V + 5 V = 125 V on VIN, 120 V on GND — the device still sees 5 V across its supply rails).

### Summary Checklist for Ground-Triggered Input Wiring

1. Connect switch/sensor output to the input pin.
2. Connect the other side of the switch to chassis ground.
3. Add a pull-up resistor from the input pin to the appropriate voltage rail to prevent floating.
4. Optionally add a series or clamp diode for reverse polarity protection.
5. Ensure the chassis ground used is bonded to the ECU power ground (a common ground plane).
