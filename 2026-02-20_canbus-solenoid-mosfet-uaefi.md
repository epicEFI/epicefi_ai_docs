# CAN Bus Solenoid Control and MOSFET Output Strategies on uaEFI/mega2560

*Source: rusEFI Discord, 2026-02-20 | Channel: 1470466409602744541*
*Contributors: @ggurov, @Robb235*

## Summary

Basic solenoid control functionality has been implemented in the rusEFI/epicEFI system and operates over CAN bus communication between a microcontroller and a Mega2560. For PC (pressure control) and TCC (torque converter clutch) PWM outputs, options are to run them directly off the main board or mirror the PWM signal over CAN bus via the mega_epic firmware. An alternative approach for driving solenoids using ignition outputs with external MOSFETs was also discussed.

## Details

### CAN Bus Solenoid Implementation

- Basic solenoid functionality is confirmed working over CAN bus to a Mega2560.
- This enables solenoid control from a remote microcontroller communicating via CAN.

### PC/TCC PWM Output Options

Two approaches are available for PC (pressure control) and TCC (torque converter clutch) PWM:

1. **Run PWM off the main board directly** - the straightforward approach.
2. **Mirror PWM over CAN bus via mega_epic firmware** - requires firmware modification to grab the PWM signal over CAN bus; described as a "hack" but functional.

### Driving Solenoids via Ignition Outputs and External MOSFETs

- @Robb235 raised the idea of using uaEFI ignition outputs with MOSFETs soldered on to drive solenoids.
- Key limitation noted: solenoids typically require **high-side driving**, which ignition outputs (low-side switches) do not natively support.
- @ggurov confirmed that ignition outputs can be used to drive **large external MOSFETs**, which could then be configured for high-side switching of solenoids.
- This approach allows repurposing available ignition output channels for solenoid drive applications where the board does not have dedicated high-side outputs.
