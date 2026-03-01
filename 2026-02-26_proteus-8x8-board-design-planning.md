# rusEFI 8x8 Generic Board Design: Proteus Footprint in p01 Case with 3xCAN

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @sangawku, @hydrocarbon82, @ggurov*

## Summary

SanGawku announced plans to design a new generic 8x8 (8 injector / 8 ignition) rusEFI-compatible board based on the Proteus footprint. Key design targets include fitting inside a GM p01 ECU case, providing 3x CAN bus interfaces with a jumper for GMLan, and enough output channels to control a 4-speed automatic transmission. The Proteus footprint was confirmed to physically fit inside the p01 case, with connector holes landing just inside the large inner cavity.

## Details

### Board Specification Targets

- **I/O**: 8 injector outputs, 8 ignition outputs ("8x8 generic")
- **Footprint**: Based on Proteus board dimensions
- **Form factor**: Designed to fit inside a GM p01 ECU case
  - The Proteus footprint fits in the p01 case; connector holes land just inside the large inner cavity of the case
- **CAN bus**: 3x CAN interfaces, with a jumper to configure one for GMLan
- **Outputs**: Sufficient outputs to control a 4-speed automatic transmission

### Transmission Control Output Requirements

Discussion of 4L80E (4-speed automatic) solenoid control requirements:
- GM 4L80E requires approximately 5-6 solenoids total:
  - 3 gear selection solenoids
  - 1 line pressure solenoid (requires PWM at ~614 Hz)
  - 1 torque converter clutch (TCC) lockup solenoid
- SanGawku estimated 5 outputs sufficient; Hydrocarbon noted 6 as a safe count for a full V8 4L80E setup

### PWM for Transmission Control

- mega_epic (Arduino Mega 2560 + MCP2515 CAN) firmware does not currently handle PWM well
- Suggested architecture for transmission control with existing hardware:
  - Use a dedicated onboard PWM driver for the line pressure solenoid (614 Hz requirement)
  - Use mega_epic for gear select solenoids and sensor inputs
  - Repurpose spare digital inputs as outputs where possible; dc2 spare outputs on uaefi also usable
- Reference for DIY standalone trans controller: https://sites.google.com/site/sloppywiki/everything-ls/cheap-stand-alone-trans-controller

### mega_epic TCU Notes

- mega_epic hardware: Arduino 2560 + MCP2515 CAN board
- TCU code for 4L80E was developed and test-driven (reportedly 4 hours of driving)
- Operates as a TCU over CAN bus (mega-epic firmware)
- GitHub: https://github.com/ggurov/MEGA_EPIC_CANBUS

### Output Sufficiency on Existing uaefi Hardware

Hydrocarbon confirmed that with mega_epic firmware, uaefi has enough I/O for a full V8 + 4L80E by:
- Repurposing digital inputs as outputs (where circuit allows)
- Using the spare dc2 outputs
- Offloading PWM-critical channels (line pressure solenoid) to a dedicated onboard driver

### Context: Why 8-Channel Minimum Matters

Several community members noted that 6-channel (injector/ignition) boards are too limited for practical use with common project car engines. The LS V8 platform alone has a large user base and requires 8 channels minimum. The 8x8 design directly addresses this gap.
