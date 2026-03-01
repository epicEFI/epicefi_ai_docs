# Fuel Pump Computer (FPC) Bypass Options with Standalone ECU

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov, @SanGawku*

## Summary

Toyota/Lexus vehicles equipped with a Fuel Pump Computer (FPC) use a 5V PWM signal to modulate fuel pump speed. When converting to a standalone ECU, the FPC can be bypassed in two documented ways: having the standalone ECU drive a 12V relay through the existing 5V PWM signal line, or hijacking the FPC signal wire so the standalone ECU drives the relay directly. A third approach (observed in the field) is jumpering the FPC input so it runs the pump at full voltage continuously without any ECU control.

## Details

### Stock FPC Operation

The Toyota Fuel Pump Computer (FPC) controls pump speed via:
- Input: **5V square wave PWM signal** from the OEM ECU
- Output: Variable pump voltage, modulating pump speed proportionally to the PWM duty cycle
- The FPC is a 5-wire controller

### Bypass Option 1: Standalone ECU Drives a 12V Relay via 5V PWM Line

The standalone ECU (rusEFI/epicEFI) outputs a 5V square wave on the FPC signal wire, but instead of controlling the FPC's speed output, the signal is used only to switch a 12V relay:

```
Standalone ECU --> 5V PWM signal --> Relay coil (via appropriate circuit)
                                  --> 12V relay contacts --> Fuel pump (full voltage)
```

- Pump runs at full voltage whenever relay is energized
- PWM duty cycle controls relay on/off state, not pump speed
- Simple and reliable if variable pump speed is not needed

### Bypass Option 2: Hijack the Signal Wire, Standalone ECU Drives Relay Directly

The OEM FPC signal wire is repurposed: the standalone ECU takes over the wire entirely and drives a relay coil directly over the same wire path.

```
Standalone ECU --> hijacked wire --> 12V relay coil
                                  --> relay contacts --> Fuel pump
```

- FPC is effectively removed from the circuit
- Standalone ECU has full control of relay switching
- Requires understanding the existing wire routing

### Observed Field Configuration (Not Recommended)

One SC300 being discussed had the FPC bypassed by "just adding a wire" — a jumper was used to send a constant 5V signal to the FPC controller input. Result: the FPC runs the fuel pump at **full voltage continuously** regardless of engine state. The owner noted this was not a permanent solution. The practical risk is that the pump runs whenever the circuit is powered, with no ECU control to shut it down.

### Pump Shutdown Behavior

With a properly configured relay-based bypass, the pump shuts off when the relay is de-energized (ECU removes the control signal). With the "constant 5V jumper" workaround, the pump runs continuously while the circuit has power.

### Recommendation

If the FPC is to be bypassed on a standalone ECU build:
1. Verify which bypass approach the previous owner used before assuming any behavior
2. Option 1 (relay driven by 5V PWM from ECU) is the simplest to implement and diagnose
3. Understand that running the fuel pump at full voltage continuously may reduce pump life and always delivers more fuel pressure than the regulator needs, which is generally acceptable but wastes power
