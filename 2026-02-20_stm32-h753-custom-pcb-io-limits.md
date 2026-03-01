# STM32 H753 / Nucleo-144 Custom PCB for epicEFI: Pin Mapping and I/O Limits

*Source: rusEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @punisher496, @ggurov*

## Summary

A new community member considering a custom PCB build around an STM32H753 Nucleo-144 board asked about epicEFI compatibility, pin flexibility, analog input counts, and staged injector support. @ggurov provided a detailed breakdown of I/O constraints on STM32H7 devices and recommended an alternative H7 adapter board better suited to custom ECU development than the Nucleo-144.

## Details

### Compatibility: STM32H753 Nucleo-144 with epicEFI

- epicEFI **will run on the H753 Nucleo-144** if the user insists on that platform.
- However, @ggurov recommends against it for custom PCB work for several reasons:
  - The Nucleo-144 has extra onboard peripherals that are unnecessary for ECU use.
  - Its morpho header pinout is non-standard for the ECU community.
  - Voltage dividers and op-amps for analog signal conditioning are absent and must be added externally.
  - The H753's Ethernet peripheral (which could work if enabled in firmware) **takes over several analog input lines**, reducing available ADC channels.
  - Ethernet offers little practical advantage over USB for ECU tuning communication.

### Recommended Alternative: DCwerx STM32-H7 Adapter Board

@ggurov specifically recommended:
- **DCwerx STM32-H7 Adapter Board** (Arduino Mega 2560 footprint): https://dcwerxtuned.com/product/stm32-h7-adapter-board-arduino-mega-2560-footprint/
- Uses the **Arduino Mega 2560 / Speeduino-compatible pinout** — any Speeduino board KiCad file can be used as a starting point and modified.
- Includes onboard **voltage dividers and op-amps** for analog signal conditioning.
- Cleaner platform for custom ECU carrier board development.

### Pin Mapping Flexibility in epicEFI

All STM32 GPIO pins are software-mappable in epicEFI with some constraints:

| Function | Capability |
|---|---|
| Digital output (PWM) | Any GPIO pin can be configured as a PWM output |
| Digital input (level) | Any GPIO pin can be used as a slow-poll level input |
| Event/trigger inputs | 16 total (hardware EXTI limitation on all STM32 devices) |
| Analog inputs | 16 total (ADC1 and ADC2 only; **ADC3 is reserved** and not available) |

- The 16-trigger-input limit is an **STM32 EXTI hardware constraint** — it applies to all STM32 variants, including the H753. A Nucleo-144 provides no advantage here.
- ADC3 being reserved means users should not plan analog circuits around those specific MCU pins.

### Staged Injector Support (8+8 Configuration)

@punisher496 asked about using 8 primary and 8 secondary staged injectors:
- epicEFI supports staged injection; no specific output pins are hardcoded for secondaries — any assignable output can be used.
- For 16 total injector channels, a **custom external injector driver board** may be required (the user confirmed they were open to this).
- **Staging blend cap**: In TunerStudio, the staging blend was previously capped at 90%. @ggurov confirmed this was **fixed the same day** (2026-02-20) — the limit now goes to 100%.

### EGT (Exhaust Gas Temperature) Inputs

For 8x EGT channels, the supported approach in epicEFI is via **SPI thermocouple chips** (e.g., MAX31855 or equivalent). The epicECU reference design includes 8x SPI EGT inputs, confirming this is a tested and supported configuration.

### Recommended Starting Resources for Custom H7 ECU PCB

- **Proteus ECU**: https://github.com/mck1117/proteus — start here for a reference H7 ECU design.
- **UA4C boards**: https://github.com/turboedge/SpeedyBoards/tree/master/UA4C — additional community board designs.

### Suggested Feature Set Feasibility

@punisher496's target feature set (8 ignition, 8 primary injectors, 8 secondary injectors, dual DBW, alternator control, multiple fans, 2 water pumps, 8 EGTs, misc sensors) was assessed as achievable with epicEFI on an H7 platform, provided:
- A custom injector driver board handles the 16-channel injector requirement.
- EGTs are handled via SPI daisy-chain (not individual analog inputs).
- Analog inputs are planned around ADC1/ADC2 only (16 channels maximum).
