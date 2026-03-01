# GPPWM for Generic Pressure and Auxiliary Load Regulation

*Source: rusEFI Discord, 2025-11-21 | Channel: general-tuning (1401895238481744052)*

*Contributors: Joesphan, ggurov*

## Summary

Users asked about implementing generic closed-loop pressure or load regulation in epicEFI — specifically for a brake booster vacuum pump and a CO2 pressure tank. The existing General Purpose PWM (GPPWM) outputs were identified as the mechanism for open-loop duty-cycle control of such loads. For actual closed-loop pressure regulation (maintaining a setpoint), a PID-based approach would be needed beyond standard GPPWM tables.

## Details

### Use Case

- Joesphan's request: *"can we get some generic regulation controls? for ex like my brake booster pump control, or pressure tank for CO2"* — also mentioned wastegate as another example.
- These loads need to be driven as a function of some feedback variable (pressure) to maintain a setpoint.

### GPPWM as the Current Solution

- ggurov confirmed that **GPPWM (General Purpose PWM)** outputs, configured with Y-axis based on the relevant sensor (e.g., pressure sensor reading), can drive the output duty cycle as a table lookup.
- This provides open-loop duty-cycle scheduling: the PWM output varies based on a configured axis value (engine condition, sensor value, etc.).
- ggurov: *"so that's gppwm with Y based on the thing you're trying to control?"*

### Limitation: No Native Closed-Loop Pressure PID

- Joesphan noted that GPPWM alone may not be sufficient: *"yeah I suppose, but might need PID or something smarter"*
- True closed-loop regulation (hold a target pressure) would require a PID controller using the pressure sensor as feedback — this is not directly implemented as a generic "pressure regulator" block in current epicEFI.
- As of this discussion, users requiring closed-loop auxiliary pressure control would need to implement this through custom GPPWM scheduling tuned to approximate the desired behavior, or wait for a dedicated generic PID output feature.

### Suggested Workaround

- Configure a GPPWM output with the relevant pressure/vacuum sensor on the axis.
- Set the duty cycle table so that the output PWM drives the pump/valve to maintain approximate pressure across the operating range.
- For a brake booster vacuum pump: use manifold vacuum or a dedicated vacuum sensor as the axis; schedule duty cycle to ramp up when vacuum is low.
