# Cruise Control Implementation in rusEFI/epicEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @ggurov, @YeOldePirate, @Chemex-joshseek*

## Summary

A community contributor (@ggurov) developed a cruise control feature for rusEFI/epicEFI. The discussion covered the implementation approach (P-factor from a PID controller driving throttle position), recommended additional features (brake light disengage pin, RPM hold), and how the closed-loop idle RPM control already in the firmware relates to this work. Additional auxiliary outputs for fan and electric water pump PWM control were also mentioned.

## Details

### Cruise Control Implementation

The cruise control feature was implemented using a **proportional (P) factor from a PID controller**:

> "yeah... my cruise implementation is basically P factor from PID"

This means the throttle correction is proportional to the error between the target speed and the actual speed. A full PID implementation (with integral and derivative terms) could be added for improved steady-state accuracy and transient response, but the P-only approach provides a functional baseline.

### Relationship to Closed-Loop Idle Control

The existing rusEFI/epicEFI firmware already includes **closed-loop idle control at a set RPM**. This is distinct from cruise control but uses a similar feedback approach:

> "It's closed loop idle at set rpm"

The suggestion to build an "RPM hold" feature was discussed — this would maintain a specific engine RPM (e.g., 3000 RPM) under varying load conditions, useful for scenarios like welding from a PTO or running accessories:

> "then you can basically get perfect voltage output, when people start welding or whatever it will just bring the TPS up to keep that 3000rpm"

This RPM hold feature would use closed-loop throttle control to maintain target RPM regardless of electrical load.

### Recommended Feature: Brake Light Disengage Pin

A community member suggested adding a **brake light disengage pin option** to the cruise control implementation:

> "But you should really add a brake light disengage pin option"

This would allow specifying an ECU input pin connected to the brake light switch, so that pressing the brake pedal automatically disengages cruise control — a standard safety feature in production cruise control systems.

### Auxiliary Outputs: Fan and Water Pump PWM

In the same discussion context, additional outputs available on the ECU were noted as suitable for PWM control of:
- **Cooling fan** (variable speed via PWM)
- **Electric water pump** (variable flow via PWM)

> "then use the other outputs for fan and electric water pump pwm control"

rusEFI/epicEFI boards typically have multiple general-purpose PWM-capable outputs (GP-PWM) that can be configured for these purposes without requiring additional hardware.

### Implementation Notes

- Cruise control input source: PPS (Pedal Position Sensor) for driver intent, ETB for throttle actuation.
- PID P-factor cruise control is a starting point; I and D terms can improve performance.
- Brake light switch input is needed for safety disengagement.
- Closed-loop idle RPM control (already in firmware) can serve as the basis for an RPM hold feature.
