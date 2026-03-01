# Configuring Analog Inputs in YAML and Compiling Custom epicEFI Firmware

*Source: epicEFI Discord, 2026-02-27 | Channel: 1356732771325968630*
*Contributors: @ggurov, @Detonation*

## Summary

A developer working on custom dev-boards (OpenLogicEFI) asked about configuring pin assignments for a custom epicEFI board variant. ggurov walked through the YAML configuration format for analog inputs, digital I/O, and how to clean up unused entries. ggurov also shared how to compile a custom firmware variant using a Docker-based build container, usable on Linux or WSL2. The discussion also touched on STM32F407 pin count limitations and the dcwerx STM32-H7 adapter board.

## Details

### YAML Configuration: Analog Inputs

To configure an analog input in the epicEFI board definition YAML:

```yaml
- class: analog_inputs
  meta: M144_A1
  pin: 55
  ts_name: PA1
  type: av
```

- **`class`**: `analog_inputs` — defines this as an analog voltage input.
- **`meta`**: hardware reference label (e.g., `M144_A1` for connector M144, pin A1).
- **`pin`**: MCU pin number (e.g., `55` = PA1 on STM32).
- **`ts_name`**: the name displayed in TunerStudio for this input. Change this to a meaningful label.
- **`type`**: `av` = analog voltage.

### YAML Configuration: Digital I/O (Switch Inputs / Outputs)

```yaml
- class: [switch_inputs, outputs]
  id: [ B2, B2 ]
  ts_name: PB2
```

- Change `ts_name` to a descriptive label matching your wiring.
- Remove entries for any pins that are **not wired or not present** in the physical build to avoid confusion.

### Cleaning Up Unused Entries

- Delete YAML entries for pins/peripherals that are not connected or do not exist on your hardware.
- Keeping unused entries can cause TunerStudio to show phantom inputs/outputs, making configuration harder.

### Compiling Custom Firmware

The recommended build environment is a Docker container maintained at:
`https://github.com/ggurov/rusefibuildcontainer`

- The container includes all required toolchains and dependencies.
- Works on native Linux or via WSL2 on Windows.
- Once the container image is built, iterative firmware compilation is straightforward.
- The workflow: customize the board YAML file → compile → produce a new firmware variant → submit for inclusion in official builds if desired.

### STM32F407 Pin Count Notes

- The STM32F407 in a **100-pin LQFP** package, after removing unused peripherals (USB OTG, DAC, etc.), is short only one or two pins for a full-featured EFI ECU.
- Moving to a **144-pin LQFP** package of the F407 provides extra pins with headroom.
- The dcwerx **STM32-H7 adapter board** (same Arduino Mega 2560 form factor and connector placement) is an alternative that uses an H7 and has more resources: `https://dcwerxtuned.com/product/stm32-h7-adapter-board-arduino-mega-2560-footprint/`

### Level Shifter Considerations for Custom Boards

- Many external gate drivers and MOSFETs (common in aftermarket EFI wiring harnesses) require a **5V gate signal**, but STM32 GPIO is 3.3V.
- Custom board developers should include **level shifters** for selected output channels if the downstream hardware requires 5V logic.
- This is relevant when designing boards compatible with existing wiring harnesses built around 5V-logic ECUs.
