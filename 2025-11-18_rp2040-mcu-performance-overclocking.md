# RP2040 MCU Performance: Table Loading, Overclocking, and MCU Comparison

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @ggurov, @Mitch*

## Summary

Mitch reported benchmark results for loading full 16x16 lookup tables on the RP2040-based epicEFI hardware, discussed the RP2040's overclocking headroom, and compared it to alternative MCUs including the RP2350 and the 600 MHz Teensy.

## Details

### 16x16 Table Loading Performance

Loading six full 16x16 tables on the RP2040 takes only 21 ms. Mitch noted this is acceptable performance for the current workload.

> "testing with *full* 16x16 tables, currently it's loading 6 of them in 21ms. Not too bad." — @Mitch

### RP2040 Overclocking

The RP2040's base clock is 133 MHz. Mitch has successfully overclocked the chip to 250 MHz in the epicEFI firmware:

> "These things overclock like crazy. Base clock on rp2040 is 133Mhz, I currently have it overclocked to 250Mhz." — @Mitch

### MCU Comparison: RP2040 vs RP2350 vs Teensy

When asked what the most capable MCU would be for running rusEFI/epicEFI, Mitch identified the following options:

- **RP2350**: The current best practical upgrade path. It is a drop-in replacement for the RP2040, making it straightforward to adopt without a complete board redesign.
- **Teensy (600 MHz)**: Theoretically the most powerful option, running at 600 MHz. However, it is not a drop-in replacement and would require significant hardware changes.

> "rp2350. Drop-in replacement for rp2040." — @Mitch

> "Nothing like that 600Mhz teensy" — @Mitch

### Practical Notes

- The RP2040's overclocking capability means there is headroom beyond the stock 133 MHz without hardware changes.
- If computational demand grows, the RP2350 is the logical next step given its pin and peripheral compatibility with the RP2040.
