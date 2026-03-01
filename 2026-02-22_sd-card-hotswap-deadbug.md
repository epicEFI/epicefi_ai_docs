# SD Card Integration: Tune Hotswap, Deadbugging, and Capacity

*Source: rusEFI Discord, 2026-02-22 | Channel: 1411925674498719775*
*Contributors: @ggurov, @YeOldePirate, @SanGawku*

## Summary

A detailed discussion about epicEFI's SD card tune-hotswap feature, how to add an SD card to a board that lacks a socket (via deadbugging), the SPI interface pins involved, formatting requirements, and minimum capacity for non-logging use. The context also covers a memory optimisation finding where reducing a blocking-factor segment frees 2x that amount in RAM.

## Details

### Tune Hotswap via SD Card

epicEFI writes tune data to an SD card. On boot, if the SD card contains a newer tune than what is in flash, the ECU automatically loads the SD card tune ("hotswaps" it).

- This enables persistent long-term fuel trims and planned features: virtual gas tank, odometer, engine hour meter, rev limit counter
- Feature is active for STM32F4 / F7 targets ("write while running is in for f4/f7 now")

### Adding an SD Card to a Board Without a Socket

If a board does not have an SD card socket populated, an SD card adapter can be wired directly to the SPI bus:

- **SPI bus**: SPI3
- **Additional pin needed**: any free digital GPIO for chip select (CS)
- Example available pin mentioned: PC9 for CS
- Other available SPI pins on the tested board: C12, C11, C10
- The SD card does not need to be removable — soldering directly to the µSD card pads is viable ("deadbug" style)
- If space is very tight, ggurov suggested a CAN bus bridge as a third-tier fallback for SD data

### SD Card Formatting

- No special formatting required; factory-formatted cards work out of the box
- The STM32 has provisions to reformat the card if the format is incorrect
- Standard FAT formatting (as shipped) is sufficient

### SD Card Capacity for Non-Logging Use

- **1 GB is sufficient** if the logging feature is disabled
- With logging disabled, the SD card is only used for tune writes ("online writes")
- There is a firmware option to turn off logging entirely

### ECU Memory Optimisation: Blocking Factor Segment

ggurov reported a finding about the blocking factor memory segment on F4:

- The blocking-factor segment was found to be **duplicated twice** in memory
- Reducing the segment size by **1500 bytes** recovers **3000 bytes of RAM** (2x multiplier due to duplication)
- This gives "some life back to f4" — the STM32F4 is RAM-constrained
- More transfer chunks are required as a side effect, but the RAM savings are worthwhile
- SanGawku noted a related issue: his blocking factor was literally 3 bytes over the size limit; making it a power-of-2 would resolve his specific case

### Configurable CAN Broadcast

In the same session, YeOldePirate requested configurable CAN broadcast (similar to ECUMaster's approach where CAN packet layout is fully user-defined). ggurov confirmed this was already implemented ("done"), including a Fahrenheit-to-Celsius toggle for broadcast temperature units.
