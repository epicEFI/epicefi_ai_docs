# Display Dash Options for STM32F1-Based rusEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @GGGI, @Joesphan*

## Summary

@GGGI asked about compatible display dash options for an STM32F1-based rusEFI build, intending to use a Nextion 3.5" display. @Joesphan suggested looking at LVGL-based displays available on AliExpress or Amazon.

## Details

### Context

- Target hardware: **STM32F1**-based rusEFI ECU
- Original display candidate considered by user: **Nextion 3.5 inch**
- Alternative suggestion: **LVGL** displays

### LVGL as a Display Option

LVGL (Light and Versatile Graphics Library) is an open-source embedded graphics library used on a variety of small LCD/TFT display modules. Displays with LVGL support can typically be found on:

- **AliExpress**
- **Amazon**

@GGGI asked whether these displays come pre-programmed for STM32 or require custom firmware development. No definitive answer was provided in this snippet — further research into LVGL integration with rusEFI on STM32F1 hardware would be needed.

### Notes

- Nextion displays use a proprietary serial protocol and their own IDE for UI design, which makes them straightforward to prototype with but potentially limiting for deeper integration.
- LVGL-based displays require more custom code but offer greater flexibility in UI design.
- The STM32F1 is a relatively resource-constrained platform; display capabilities may be limited by available RAM/flash and processing power.
