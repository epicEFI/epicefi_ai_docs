# TCU Solenoid Control and Gear Selector Signal Conditioning

*Source: rusEFI Discord, 2026-02-21 | Channel: 1470466409602744541*
*Contributors: @ggurov, @Robb235, @SanGawku*

## Summary

Discussion covers TCU (Transmission Control Unit) integration with rusEFI/epicEFI, including: the firmware parameter definitions for EPC (Electronic Pressure Control) and torque converter duty, hardware signal conditioning for driving transmission solenoids from microcontroller outputs, gear selector voltage step-down, and considerations for implementing a towing mode using a second shift table on the STM32F4 platform.

## Details

### TCU Firmware Parameters

The following `int8_t` configuration fields are used in rusEFI/epicEFI for TCU pressure control:

```
int8_t tcu_pressureControlDuty;"TCU: EPC Duty";"%",1,0,0,100,0
int8_t tcu_torqueConverterDuty;"TCU: TC Duty";"%",1,0,0,100,0
```

- `tcu_pressureControlDuty`: Controls the Electronic Pressure Control (EPC) solenoid duty cycle. Range: 0–100%, scale 1, offset 0.
- `tcu_torqueConverterDuty`: Controls the torque converter lockup clutch solenoid duty cycle. Range: 0–100%, scale 1, offset 0.

These are `int8_t` (8-bit signed integer) fields, giving a practical range of 0–100 with 1% resolution.

### Solenoid Driving: High-Side with Signal Conditioner Board

Transmission solenoids in @Robb235's application are **high-side driven** at 12V, but the microcontroller (rusEFI "mega" board) outputs 5V logic signals.

A custom **transmission signal conditioner board** was built to interface the two:
- Accepts 5V output signals from the rusEFI mega board.
- Uses **MOSFETs** to switch 12V to the solenoids.
- Allows the ECU to control solenoids without direct high-voltage output capability.

This board is used alongside a dedicated adapter board for the transmission integration.

### Gear Selector Signal Conditioning

The gear selector (PRNDL switch) in @Robb235's vehicle outputs **12V** to the stock ECU for each selector position.

To interface with rusEFI/epicEFI (which expects 0–5V analog inputs on the "mega" board):
- **Resistor voltage dividers** are used to step down the 12V gear selector signals to approximately **5V**.
- These divided signals are then fed into analog input pins on the rusEFI board.

### Towing Mode: Second Shift Table Consideration

@ggurov raised the need to implement a towing mode in the TCU calibration. @Robb235 asked whether this should be done via a **second shift table** and whether adding a second shift table would consume too much memory on the **STM32F4** (F4) platform.

- The F4 platform has constrained RAM/flash, so adding a full second shift table may be marginal.
- The conversation was ongoing — no final implementation decision was reached.

### Key Takeaways

- EPC and TC solenoid duty cycle are 0–100% `int8_t` fields in the rusEFI/epicEFI TCU configuration.
- High-side 12V solenoid driving requires a signal conditioner board with MOSFETs when using 5V MCU outputs.
- Gear selector 12V signals must be divided down to ~5V using resistor dividers before connecting to rusEFI analog inputs.
- Towing mode via a second shift table is under consideration but may be constrained by F4 memory limits.
