# MEGA144H7 (DCwerx Green Board) Installation Notes on Speeduino

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @kitaji_SR20*

## Summary

A user documented their process of installing the DCwerx MEGA144H7 board onto a Speeduino platform, sharing reference notes for others in similar situations. Two hardware unknowns were identified: the SPI CS pin for the onboard SD card and the CAN module pin layout.

## Details

### Hardware

- **Board:** DCwerx MEGA144H7 (green board variant)
- **Target platform:** Speeduino

### Known Unknowns (Without Schematic)

Two pin assignments were uncertain at time of writing:

1. **SPI CS pin for onboard SD card** — The chip-select pin used by the SPI-connected SD card is not confirmed. Without a DCwerx schematic, this must be probed or inferred from firmware source.

2. **Physical CAN module pin layout** — Likely uses PD0 and PD1 (common STM32 USART/CAN pin assignments), but not confirmed without schematic access.

### Notes

- A DCwerx schematic would resolve both uncertainties; if available, consult the hardware documentation or contact DCwerx directly.
- The original notes were written in Japanese; non-Japanese readers should be aware the source material may read slightly differently in translation.
- Markdown sharing of installation notes in Discord was noted as a suboptimal format for this type of documentation.
