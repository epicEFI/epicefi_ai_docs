# RP2040/RP2350 ECU Hardware: USB Serial Communication, ADC Pins, and SPI ADC Expansion

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @mitch0s, @ggurov, @joesphan*

## Summary

This thread covers hardware details for an epicEFI ECU build based on the RP2040/RP2350 microcontroller. Topics include the USB serial communication capability (no Ethernet, up to 12 Mbps via the built-in USB controller), the limited number of onboard ADC pins, the plan to use an external ADC IC over SPI for additional 0–5V analogue inputs, and user-facing pin numbering abstraction.

## Details

### No Ethernet — USB Serial at Up to 12 Mbps

The RP2040/RP2350 does not have an Ethernet interface. Instead, it uses its own integrated USB controller to provide serial communication:

> "nah, but the rp2040/rp2350 has it's own usb controller that lets it communicate over 'serial' at upwards of 12Mbps apparently — more than enough if true."

This provides sufficient bandwidth for ECU tuning software communication. The hardware throughput ceiling is not the limiting factor; the bottleneck is the tuning-software's serial reading implementation:

> "The pico can actually send messages fast as, the limitation is really my implementation of reading serial on the tuning-software side of things."

### RP2040 ADC Pin Limitations

The RP2040 has only **4 onboard ADC pins**, and they are rated for **3.3V** maximum input. This is insufficient for standard automotive 0–5V analogue sensors.

### External ADC IC for 0–5V Analogue Inputs

To handle the 0–5V analogue inputs typical in automotive applications, a separate external ADC IC will be used. @ggurov achieved an additional **24 ADC channels via SPI** without using a multiplexer (MUX):

> "i don't like mux for some reason, besides. i got our 24 extra adc working over spi"

This is preferred over a MUX approach for routing additional analogue channels.

### User-Facing Pin Numbering Abstraction

To simplify the user experience, an internal mapping layer will translate user-defined pin numbers to actual hardware ADC channels:

> "Will create a mapping internally for what pins go where, the user will see them as 1,2,3,4... which correlates to ADC1, ADC2, ADC3... in the wiring pinout"

Users interact with abstract sequential pin numbers (1, 2, 3, 4...) which are mapped internally to the correct ADC channels on the external IC and onboard hardware. This mapping aligns with the physical wiring pinout documentation provided to the end user.

### Summary Table

| Feature | Detail |
|---|---|
| MCU | RP2040 / RP2350 |
| Communication | USB serial (no Ethernet), up to 12 Mbps |
| Onboard ADC pins | 4, rated 3.3V |
| External ADC expansion | Separate ADC IC, 24 additional channels via SPI |
| Analogue input range (external) | 0–5V |
| User pin numbering | Abstract sequential (1, 2, 3...) mapped to physical ADC channels |
