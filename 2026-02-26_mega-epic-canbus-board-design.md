# mega_epic CAN Bus I/O Expansion Board Design

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov, @GGGI, @ZeeKay, @SanGawku, @Joesphan, @punisher496*

## Summary

Discussion of the mega_epic CAN bus expansion board design, its hardware architecture, intended use as an I/O expander for rusEFI/epicEFI, and related topics including STM32 microcontroller pin compatibility, CPU headroom concerns with ESP32, and multi-speed CAN bus (GMLan) control.

## Details

### mega_epic Board Hardware

The mega_epic is a CAN bus I/O expansion board designed for use with rusEFI/epicEFI systems. Key hardware characteristics:

- **Base MCU:** Arduino Mega 2560 (common, widely available)
- **CAN controller:** MCP2515 (SPI-connected CAN controller IC)
- **Outputs:** MOSFET-driven outputs for load switching
- **Protection:** Enhanced protection features modeled after the ua4c board design
- **I/O density:** Fully populated with all ADC channels, approximately 16 digital inputs and 18 digital outputs
- **VSS support:** Designed to handle quad VSS (Vehicle Speed Sensor) inputs

The board was conceived as a dedicated I/O expander that connects to the main ECU via CAN, freeing the primary processor from managing dense analog/digital I/O.

### STM32 Microcontroller Pin Compatibility

There was debate about whether STM32F407, STM32F767, and STM32H743 are pin-for-pin compatible:

- **Claim:** The MEGA144 (based on STM32F407) pins are compatible with F767 and H743 variants.
- **Counterpoint:** Per datasheet review, the F407/F767/H743 are not strictly pin-for-pin swappable — some ports/pins are rearranged between families.

**Practical implication:** Verify datasheet pin mapping before substituting one STM32 variant for another on the same PCB footprint.

### ESP32 CPU Headroom Limitations

During evaluation of ESP32 as an I/O expansion processor, the CPU was found to run out of headroom under the expected workload. The STM32 was considered as an alternative, though concerns were raised about whether the STM32 has sufficient processing power to handle the full data throughput required for dense I/O with multi-speed CAN.

### ESP32-S2 as I/O Expander

An ESP32-S2 was proposed and prototyped as an I/O expansion module. Capabilities:

- 8x additional ADC channels
- 8x digital inputs (configurable as outputs — bidirectional)
- 8x digital outputs
- 8x PWM outputs delivered via CAN bus to the main ECU

The digital I/O pins were configured to be dual-purpose (either input or output depending on runtime config).

### GMLan / Multi-Speed CAN Control

A question was raised about controlling different CAN bus speed domains (high-speed CAN, medium-speed CAN, GMLan) simultaneously from a single ECU. This is relevant for GM vehicle integrations where multiple CAN networks operate at different speeds on the same vehicle.

No specific solution was documented in this thread — the question was left open regarding how to handle simultaneous high-speed, medium-speed, and GMLan control from the rusEFI platform.
