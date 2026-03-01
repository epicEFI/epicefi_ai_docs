# A340 Transmission Line Pressure Control via uaEFI H-Bridge and PWM Outputs

*Source: rusEFI Discord, 2026-02-26 | Channel: 1470466409602744541*
*Contributors: @robb235_00916, @ggurov, @punisher496_97082*

## Summary

Discussion about controlling A340 transmission line pressure solenoids (SLT+/SLT-) using uaEFI's H-bridge outputs, the limitations of available PWM outputs on the Mega_Epic board, and considerations for revising a custom A340 signal conditioner board to add the needed outputs.

## Details

### A340 Line Pressure Control Methods

The Toyota A340 transmission has two generations of line pressure control:
- **Cable-actuated (older models):** Line pressure is mechanically set via a cable connected to the throttle body. No electronic control needed.
- **ECU-controlled (later models):** Line pressure is managed electronically via **SLT+** and **SLT-** solenoid signals from the ECU.

### Controlling SLT Solenoids with uaEFI H-Bridge

@Robb235 asked whether the SLT+/SLT- solenoid signals for line pressure on later A340 models could be driven by one of uaEFI's two onboard H-bridge controllers.

Proposed wiring approach:
- Run a **fused 12V supply to SLT+**
- Run **SLT- to one of the HB- (H-bridge low-side) pins** on uaEFI to pulse the solenoid

@ggurov asked about the power supply planning for this approach, suggesting it needs to be carefully considered before implementation.

### PWM Output Limitation on Mega_Epic

@punisher496 asked about the digital output characteristics of the **Mega_Epic** board:
- "Slow" digital outputs refer to outputs with **high latency**, not necessarily a strict frequency cap.
- A frequency limit of **10 kHz** was mentioned for high-frequency PWM on these outputs.
- This limitation is relevant when trying to drive solenoids that require fast PWM (e.g., for smooth pressure control).

The limited number of available PWM outputs on uaEFI/Mega_Epic was identified as a constraint for more complex transmission control setups requiring multiple solenoid channels simultaneously.

### A340 Signal Conditioner Board Revision

@Robb235 has a custom **A340 transmission signal conditioner board**. He considered avoiding a board revision by reusing existing uaEFI outputs, but concluded that revising the signal conditioner board to accommodate the SLT solenoid requirements was the better path forward, given the PWM output shortage on the stock uaEFI hardware.

### Arduino Mega 2560 as Standalone TCU

@Robb235 was experimenting with using a **uaEFI + Arduino Mega 2560 combination running epic firmware** as a standalone TCU. The Mega 2560 would receive TPS, RPM, and VSS over the CAN bus from the epicEFI ECU and handle transmission control independently.

@GGGI confirmed this concept: the Mega 2560 controlled the transmission via the epicEFI CAN bus. This approach is a way to offload transmission logic to a dedicated microcontroller while keeping the main ECU handling engine management.

**Note:** This approach does not resolve the PWM output shortage on its own — the Mega 2560 would need sufficient PWM-capable output pins for all required solenoid channels.
