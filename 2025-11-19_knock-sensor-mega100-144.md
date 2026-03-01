# Knock Sensor Integration on Mega 100 and Mega 144

*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov, @dcwerx*

## Summary

Discussion of how to integrate a knock sensor into the rusEFI Mega 100 and Mega 144 hardware. Knock sensor support requires an external filtering circuit and must be connected to an ADC3-capable pin on the MCU, then mapped in the `knock_control` firmware module. At the time of discussion, knock sensor functionality had not yet been tested on production hardware, but a community member had already built the filtering circuit and was preparing bench testing.

## Details

### Hardware Requirements

- A filtering/conditioning circuit is required between the knock sensor and the MCU — the raw piezoelectric signal from a knock sensor cannot be connected directly to a GPIO
- The input pin must be an **ADC3** pin on the STM32 (not ADC1 or ADC2); not all exposed header pins qualify
- @ggurov: "it has to be an ADC3 pin — so MAY be able to use one of the tiny header things" (referring to small auxiliary header connectors on the Mega 100/144 PCB)

### Firmware Configuration

- The knock sensor pin must be mapped within the `knock_control` firmware module
- No dedicated "knock sensor input" is pre-mapped on the Mega 100 or Mega 144 by default — configuration is required
- @ggurov: "it needs the filtering circuit, and then we have to map it in knock_control to one of the pins there"

### Status at Time of Discussion

- Mega 100 and 144 knock sensor support was untested: "it doesn't [work] — at least we haven't tested it"
- @dcwerx was testing with a Mega 100 (STM32F4 variant), with a Mega 144 H7 available as well
- @dcwerx had completed the filtering circuit and hardware side, and had it "hooked up a sim" (bench simulation)
- The next step was confirming proof-of-life signal and verifying firmware configuration:
  - Need RPM input on the bench
  - Need filtering circuitry connected
  - Need TunerStudio-side configuration sorted

### Hardware Tested

- Mega 100 (STM32F4) — primary test target
- Mega 144 H7 (STM32H7) — secondary test target
- Both boards are based on a TurboEdge rework design
