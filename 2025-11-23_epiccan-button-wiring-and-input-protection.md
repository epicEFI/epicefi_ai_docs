# epicEFI CAN Button Wiring: "Button IN" vs "Button INPUT" and Input Protection

*Source: rusEFI Discord, 2025-11-23 | Channel: 1411925674498719775*
*Contributors: GGGI (@.gunadi), Walter R. (@walterronny)*

## Summary

Clarification of the two button-related terminals on the epicEFI Mega CAN board: "Button IN" is the physical switch connection to ground, while "Button INPUT" is the signal line going into the Arduino Mega. The Arduino Mega's built-in protections are sufficient for this input.

## Details

### Terminal Definitions

- **Button IN**: Connect the physical button between this terminal and ground. This is the switch side of the circuit.
- **Button INPUT**: Connect this to the Arduino Mega input pin. This is the signal side that the Mega reads.

The distinction is important: the external button shorts "Button IN" to ground, and "Button INPUT" carries that logic level to the Mega.

### Input Protection

Walter R. raised a question about whether additional external protection is needed. The Mega already has internal protection mechanisms on its GPIO pins (ESD protection diodes and current-limiting). For a simple button-to-ground input, the Arduino Mega's internal protections are sufficient — no additional external protection circuitry is required for a standard momentary switch connecting the pin to ground.

### Context

This discussion arose in the context of the epicEFI Mega CAN board button input. A related note (fast335xi, thread 113) mentions that the rusEFI hardware repository contains a CAN button design without external resistors so that the resistor selection can be left to the TCU/application.
