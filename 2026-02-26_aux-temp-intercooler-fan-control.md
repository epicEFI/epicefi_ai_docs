# Auxiliary Temperature Sensor for Air-to-Water Intercooler Fan Control

*Source: epicEFI Discord, 2026-02-26 | Channel: 1401895238481744052 (general-tech)*
*Contributors: @Tuga, @BossTheTuga, @ZeeKay*

## Summary

Controlling intercooler fans and water pumps based on intercooler coolant temperature (rather than engine coolant temperature) requires an auxiliary temperature sensor feeding a logic output. The current workaround uses an aux temp sensor with a logic output rule (`temp > threshold → output ON`). A feature request exists to expose an analog input source selector directly in the fans and water pump menus, along with configurable hysteresis.

## Details

### Use Case

Air-to-water intercooler systems have their own coolant loop (separate from engine coolant). The intercooler fans and water pump should respond to the intercooler coolant temperature, not the engine CLT. Without a separate sensor input and dedicated control logic, the fan/pump will either run on engine CLT (wrong temperature source) or not at all.

### Current Workaround

1. Wire a temperature sensor to an available auxiliary analog input
2. Configure an aux temp sensor in the ECU for that input
3. Create a **Logic Output** rule: `if aux_temp > X°C → enable output`
4. Assign the logic output to the fan relay or water pump output

This works but requires using the Logic Outputs section rather than the Fan/Water Pump menu, which is less intuitive.

### Proposed Enhancement (Feature Request)

- Add an **analog input source selector** to the Fan menu (similar to how CLT is currently hardcoded as the source)
- Add configurable **hysteresis** to prevent rapid on/off cycling at threshold temperatures
- This would allow intercooler fans and water pumps to be configured in the same menu as engine cooling, with any aux temp sensor as the trigger source

### Hysteresis Consideration

Without hysteresis, a fan triggered at exactly the threshold temperature will chatter rapidly as the temperature oscillates around the setpoint. Hysteresis (e.g., ON at 45°C, OFF at 40°C) prevents this. The current logic output approach requires the user to implement this manually if needed.

## Notes

- Aux temp sensor inputs support the standard NTC thermistor calibration tables used for engine CLT/IAT sensors
- Logic outputs are a functional interim solution until dedicated menu support is added
