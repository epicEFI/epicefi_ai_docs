# STM32F7 Pin Conflict: Enabling Ethernet Eliminates 8 ADC Pins

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: Joesphan (@joesphan)*

## Summary

On the STM32F7 microcontroller, enabling the Ethernet peripheral requires sacrificing 8 ADC (analog input) pins due to pin multiplexing conflicts. This is a hardware-level constraint of the STM32F7 silicon that affects ECU hardware design decisions. Other communication buses on the STM32F7 do not conflict with each other in the same way.

## Details

**The constraint:**
- STM32F7: Ethernet + 8 ADC pins share the same physical MCU pads.
- If Ethernet is enabled in the hardware design, those 8 ADC pins cannot be used as analog inputs.
- This is an STM32F7-specific silicon limitation, not a firmware or configuration issue.

**Context — why this matters for ECU design:**
- ECU applications require many analog inputs (TPS, MAP, CLT, IAT, wideband, fuel pressure, etc.).
- Losing 8 ADC channels to add Ethernet is a significant tradeoff for a typical ECU design.
- On STM32F7-based boards (e.g., Proteus F7), hardware designers must choose between full ADC complement or Ethernet connectivity.
- Other peripherals (CAN, SPI, I2C, UART) do not have this conflict — only Ethernet vs. ADC.

**Contrast with future directions:**
- Joesphan noted that moving to a TriCore (Infineon Aurix-class) MCU would resolve this class of problem: the multiple independent cores provide better peripheral isolation, ensuring a CAN bus fault (or Ethernet peripheral conflict) cannot affect ignition or injection timing.
- TriCore also provides 3 independent cores for time-critical scheduling.

**Practical guidance:**
- When designing or selecting epicEFI hardware, if Ethernet is needed (e.g., for high-speed data logging or network communication), factor in the loss of 8 ADC channels on STM32F7.
- If full ADC count is critical, omit Ethernet from the hardware design or choose a different MCU platform.
