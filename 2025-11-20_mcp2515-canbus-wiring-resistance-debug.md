# MCP2515 CAN Bus Wiring Debug: Resistance Measurement and SPI Pin Verification

*Source: rusEFI Discord, 2025-11-20 | Channel: 1434034231713075282*
*Contributors: @ggurov (ggurov), @Aboy (aboy0534_42329), @GGGI (.gunadi)*

## Summary

Troubleshooting a CAN bus interface (MCP2515-based) that was not producing a signal. Key diagnostic steps: power-down resistance measurement between CAN-H and CAN-L (expected 60 ohm with both termination resistors installed), and SPI line + chip select pin verification. The PWFusion MCP2515 library is the recommended library for this hardware.

## Details

### Symptom

User Aboy reported that a jumper wire substitution was not producing the expected CAN signal. No signal was visible on an oscilloscope connected to the CAN bus lines.

### Diagnostic Step 1: Termination Resistance Check

**Procedure:** With everything powered down, measure resistance between CAN-H and CAN-L with a multimeter.

| Measurement | Value | Status |
|---|---|---|
| Aboy's measured resistance | 120 ohm | Incorrect |
| Expected resistance (both terminators installed) | 60 ohm | Correct |

**Explanation:** A properly wired CAN bus has a 120-ohm termination resistor at each end of the bus. When both are installed and the bus is powered down, the two resistors appear in parallel across CAN-H and CAN-L, giving 60 ohm total. A reading of 120 ohm means only **one** termination resistor is present or connected.

GGGI confirmed: "Both canbus wire installed? Should be 60 ohms tho."

ggurov further clarified: the 60-ohm measurement should be taken **with everything hooked up but powered down** (i.e., the entire bus wired, controllers connected, power off).

### Diagnostic Step 2: SPI Line and CS Pin

If CAN bus termination resistance is correct and there is still no signal, scope the SPI communication lines:
- **Scope the SPI lines** (MOSI, MISO, SCK)
- **Scope the CS (chip select) line**

For the specific hardware being debugged:
- **CS pin = D9** on the Arduino/Mega platform

### Library

The recommended CAN library for MCP2515-based hardware:
- **PWFusion_Mcp2515-main.zip** (included with the board)

ggurov recommended running the example sketches from the PWFusion library as a baseline to verify the SPI communication path is functional before debugging application-level code.

### Summary of Debug Sequence

1. Power down the system.
2. Measure resistance between CAN-H and CAN-L — expect **60 ohm** with both terminators installed.
3. If incorrect, verify both termination resistors are present and connected.
4. If resistance is correct but still no signal, connect oscilloscope to SPI lines and CS pin (CS = D9).
5. Run PWFusion MCP2515 library examples to verify SPI communication.
