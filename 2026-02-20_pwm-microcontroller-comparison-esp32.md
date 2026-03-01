# PWM Capabilities of Microcontrollers Used with rusEFI (Arduino Mega, 328P, ESP32-S2)

*Source: epicEFI Discord, 2026-02-20 | Channel: 1434034231713075282*
*Contributors: @Hydrocarbon, @ggurov, @Robb235*

## Summary

Different microcontrollers used as satellite boards or expansion modules with rusEFI/epicEFI have significantly different PWM capabilities. The Arduino Mega (ATmega2560) and ATmega328P are limited to fixed-frequency hardware PWM. The ESP32-S2 offers full flexible PWM across up to 8 simultaneous pins with frequency ranges from sub-10 Hz up to MHz. Community-developed code exists to expose true PWM from these boards via the CAN bus user output interface.

## Details

### Arduino Mega (ATmega2560) and ATmega328P

- Both have hardware PWM, but the **frequency is fixed** — it cannot be changed in software without affecting the timer shared by other functions.
- Pins D44–D46 on the Mega are PWM-capable in hardware; these are marked for PWM use in some rusEFI/epicEFI pinout documentation but were not yet implemented as of this discussion.
- A community-developed **Pro Mini PWM sketch** exists that adds true PWM output capability to the 328P-based Pro Mini. This code was posted in the same channel.

### ESP32-S2

- Supports **true, flexible PWM** via the LEDC (LED Controller) peripheral.
- Frequency range: approximately **2–3 Hz up to several MHz**.
- Supports up to **8 pins simultaneously**, each with independently configurable frequencies.
- Community-developed ESP32-S2 code receives an **8-bit duty cycle value via CAN bus** (using the rusEFI user outputs CAN message) and applies it as a real PWM output on a statically assigned pin.
- A planned extension will also allow setting the PWM **frequency via CAN**, with the value stored in flash memory so it persists across power cycles.

### CAN-Based PWM Strategy

The integration strategy for using these satellite boards with rusEFI:

1. Statically assign a physical pin on the satellite board to a CAN user output channel.
2. rusEFI sends a PWM duty cycle value over CAN.
3. The satellite board ignores the on/off switching of the CAN message and instead reads the duty cycle value and drives the pin with true hardware PWM at that duty cycle.

This approach allows slow CAN update rates to control hardware PWM outputs without the choppy on/off behavior that would result from directly switching the pin at CAN message frequency.

### Crude PWM via CAN Timing

Without the above approach, the minimum achievable PWM frequency using raw CAN on/off messages is approximately **half the CAN message frequency** (since each on/off cycle requires two messages). This is considered "crude PWM" and is too slow for most actuator control applications.
