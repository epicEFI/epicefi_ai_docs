# EGT Sensors via CAN Bus: Supported Devices and DIY Approach

*Source: rusEFI Discord, 2026-02-24 | Channel: 1356732771325968630*
*Contributors: @Tuga, @ggurov, @Joesphan*

## Summary

A user needed four EGT (Exhaust Gas Temperature) sensors but their ECU only supported two direct EGT inputs. The discussion covered commercially supported EGT-to-CAN devices as well as a DIY approach using an MCU with a K-type thermocouple amplifier, sending data over CAN to the ECU. rusEFI/epicEFI supports receiving EGT data over CAN from specific supported devices, and the firmware already has internal variables that can be written via CAN to feed sensor values into the engine management system.

## Details

### Supported EGT-to-CAN Devices

The following EGT modules with CAN output are known to be supported in rusEFI/epicEFI:

- **Ecumaster EGT to CAN** module
- **AEM EGT** module (CAN output variant)

These are confirmed as supported options for connecting EGT data to the ECU over CAN.

### Problem: Exceeding ECU's Native EGT Input Count

If an ECU has fewer native EGT inputs than sensors required (e.g., 2 inputs but 4 sensors needed), the solution is to use an external EGT-to-CAN module or a DIY CAN device rather than direct wiring additional sensors to the ECU.

### DIY EGT-to-CAN Approach

For a lower-cost or more flexible solution, a DIY CAN device can be built:

1. **MCU**: Use any microcontroller with CAN support (e.g., ESP32 with CAN transceiver, or any STM32-based board)
2. **Amplifier**: K-type thermocouples require a signal conditioning amplifier (instrumentation amp). An instrumentation amplifier is sufficient -- no expensive dedicated thermocouple IC is strictly required.
   - @Joesphan: "Oh and apparently it's just an instrumentation amp you can use. Don't need the expensive deal."
3. **CAN output**: Read raw ADC values from the thermocouple amplifier in the MCU, then transmit the temperature values over CAN to the ECU.

### Integrating Custom CAN EGT Data into rusEFI/epicEFI

The rusEFI/epicEFI firmware has existing internal sensor variables that can be opened up for CAN write access:

- Variables for EGT sensor values already exist within the ECU firmware.
- The development team can expose these variables as writable CAN inputs.
- Once a variable is opened for writes, sending the appropriate CAN message causes the ECU to update the sensor value internally, making EGT data available to fuel/ignition tables and logging.
- @ggurov: "that part is super simple, variables are already in the ECU, we'd open them up for writes, you write to them, ecu updates sensors, done"

### Note on mega-epic CAN Module

The mega-epic CAN expansion module does **not** natively ship EGT values -- it ships raw ADC readings. To send EGT data via mega-epic would require using a different CAN address/variable than the standard EGT path. The core CAN integration work is straightforward once the correct target variable/address is identified.

### Hardware Recommendation for K-Type Accuracy

To get accurate K-type thermocouple readings in a DIY build, focus on the combination of:
- **MCU** with sufficient ADC resolution
- **Board** with good noise isolation/ground planes
- **Amplifier** (instrumentation amp or dedicated thermocouple amp like MAX31855) chosen for accuracy at the expected temperature range
