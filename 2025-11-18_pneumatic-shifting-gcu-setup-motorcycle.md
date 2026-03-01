# Pneumatic Shifting Setup and GCU Gear Logic for Motorcycle Applications

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat-2*
*Contributors: @kostasl, @ToyotaTerrorist, @fast335xi*

## Summary

A user integrating rusEFI on a motorcycle (kostasl) needed to configure pneumatic shifting to enable testing. ToyotaTerrorist outlined the required I/O: two digital inputs (one for torque cut management, one for solenoid trigger) and two outputs to drive the pneumatic solenoids. The thread also captures the GCU gear-shift logic: any upshift or downshift is permitted as long as the post-downshift calculated RPM would not exceed the maximum RPM limit.

## Details

### GCU Gear Shifting Logic

- **@fast335xi**: "The way it is on the GCU is you can upshift or downshift to any gear you want. As long as the downshift calculated rpm won't exceed the maximum rpm."
- The GCU calculates the expected RPM after completing a downshift and blocks the shift if that RPM would exceed the configured maximum. This prevents over-rev on aggressive sequential downshifts.

### Pneumatic Shifting I/O Requirements

@kostasl needed to wire up pneumatic shifting solenoids to rusEFI/uaEFI on a motorcycle. @ToyotaTerrorist summarized the minimal hardware required:

- **2 digital inputs**: one for torque cut/management signal, one for the solenoid activation trigger.
- **2 outputs**: to drive the two pneumatic solenoids.

Exchange:
> @kostasl: "Yes that isn't my main concern. The thing i need to figure out is the pneumatic shifting. I need to get that sorted to test rus on my bike."
>
> @ToyotaTerrorist: "You just need 2 digital inputs and 2 outputs set up for solenoids."
>
> @kostasl: "That could work. I'd have to rewire the button to two digital inputs on uaefi. One for torque management and another for solenoid activation. That's what you meant?"

### Wiring Notes

- The existing shift button needs to be rewired so that its signal is fed into two separate digital input pins on the uaEFI controller.
- One input drives the torque management (ignition/fuel cut) logic during the shift event.
- The other input triggers the solenoid activation output.
- The two outputs then control the pneumatic solenoid valves for up-shift and down-shift actuation.
