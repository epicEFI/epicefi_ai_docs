# CAN UEGO (Wideband O2) Setup and Troubleshooting on Proteus

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, Tera (teraflopping), Joesphan*

## Summary

When using CAN-connected wideband O2 (UEGO) controllers (rusEFI wideband modules) with Proteus, both a CAN lambda input and an analog O2 input can be simultaneously active if not explicitly disabled. This causes conflicts and potentially incorrect lambda readings. The rusEFI wideband tool menu in TunerStudio handles firmware updates for the wideband controllers and is separate from the lambda configuration menus. Wideband controller heater control requires the engine to be running when CAN mode is enabled.

## Details

### Dual Lambda Conflict

- epicEFI / rusEFI firmware can have **two lambda inputs active at once**: one via CAN bus and one via analog input.
- If both are enabled, the system may read incorrect or conflicting AFR values.
- **Resolution**: Disable the channel you are not using. Go to the CAN O2 menus (accessible via both the Sensors tab and the CAN tab — they are the same setting) and disable the unused lambda input.
- The TunerStudio search term "reallambda" does not exist in epicEFI builds; instead, find the setting through the **EGO (O2) menus**.

### rusEFI Wideband Tool Menu

- The "rusEFI Wideband Tools" menu in TunerStudio is inherited from the rusEFI upstream firmware. It handles **firmware update functions** for the external wideband controllers, not runtime lambda configuration.
- Lambda configuration (CAN vs. analog, enable/disable per bank) is done in the EGO/O2 sensor menus.

### Controller Heater Behavior

- rusEFI wideband CAN controllers do **not** self-heat automatically when connected via CAN — the heater is managed by the ECU firmware which requires the engine to be running to enable heating.
- If a wideband unit is powered and connected but the engine is not running, lambda will read 0 in TunerStudio.
- **Workaround for bench testing**: Enable forced heater mode in the wideband controller settings.
- When operating standalone (no CAN), the controllers handle their own heater control automatically.

### CAN Address / Hardware Index

- The CAN address used by each wideband controller is tied to a hardware index setting on the controller.
- Multiple wideband controllers on the same CAN bus must have different addresses.
- As of 2025-11-20, the firmware commit history contains work on persistent address memory across power cycles; behavior may vary by firmware version.

### Wideband O2 Sensor Compatibility

- Any wideband sensor based on the **Bosch LSU 4.9** element is compatible.
- All wideband O2 sensors are inherently heated — the element must reach operating temperature (~780°C) before producing accurate readings. Specifying "heated wideband" is redundant; all widebands are heated.
- For rear-mount turbo applications where the sensor is far from exhaust heat, this does not change sensor selection — any LSU 4.9 based unit is fine.
- The wideband controller (separate from sensor) requires knowledge of the output voltage-to-AFR curve if using an analog signal path. With CAN, this is handled internally.
