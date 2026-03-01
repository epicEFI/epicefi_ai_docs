# Upgrading Proteus MCU from STM32F4 to F7 or H7

*Source: rusEFI Discord, 2026-02-26 | Channel: 1439273357424988301 (dev/testing)*
*Contributors: @tmbryhn, @GGGI*

## Summary

The STM32F4 MCU on Proteus-based boards can be physically replaced with an STM32F7 or STM32H7 for improved performance. Hot-air rework is the confirmed method for swapping the BGA/QFP package. Community members are building custom boards based on the Proteus pinout and testing both F7 and H7 variants as drop-in replacements.

## Details

### Upgrade Path

- **Source MCU**: STM32F4 (as shipped on Proteus)
- **Target MCUs tested**: STM32F7, STM32H7
- **Rework method**: Hot-air rework station — required to remove the existing F4 and reflow the replacement MCU
- **Confirmed by**: @tmbryhn, who purchased several F7 and H7 parts specifically for board-level testing

### Custom Board Context

@GGGI is developing a custom board based on the Proteus pinout and acquired one H7 and one F4 sample for evaluation. The Proteus pinout compatibility is the key factor enabling this upgrade path without full board redesign.

### Practical Notes

- Hot-air rework is straightforward for experienced solderers but requires proper equipment and technique to avoid pad or trace damage
- Testing should be done on bench hardware before deployment in a vehicle
- SD card, knock input, and stim functionality should be verified after an MCU swap (see related SD card backup validation thread)
