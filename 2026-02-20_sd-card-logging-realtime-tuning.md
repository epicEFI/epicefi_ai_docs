# SD Card Logging, Real-Time Tune Saves, and Power-Loss Recovery

*Source: rusEFI Discord, 2026-02-20 | Channel: 1356732771325968630*
*Contributors: @ggurov, @MykkH, @Aj*

## Summary

In the rusEFI/epicEFI firmware, SD card support unlocks a significant capability: real-time tuning changes made while the engine is running can be saved to the SD card without requiring a flash update. This also means the ECU can survive a sudden power loss without losing changes that were committed during the running session. Key examples include long-term fuel trim (LTFT) corrections that accumulate while driving.

## Details

### Why SD Card Is Important in New Firmware (Mega100)

The Mega100 previously only supported saving tunes via **flash updates**, which require the engine to be off and a USB connection. With new firmware, an SD card enables:

- **Real-time tune writes while the engine is running** — changes take effect and are persisted without a flash cycle.
- **Power-loss recovery** — if the ECU loses power unexpectedly, any changes already written to SD are preserved.

> "You can save real time changes to the tune of the running vehicle via SD card, something the Mega100 couldn't do before with flash updates only." — @MykkH

### SD Card Logging and the Write-While-Running Mechanism

When SD card logging is functional, the ECU writes live state to the SD card during engine operation. This includes:

- **Long-term fuel trims (LTFT)** — adaptive corrections to the fueling map that accumulate over time.
- Other persistent calibration data that would otherwise be lost on power-off.

> "basically if sd card logging works, the ECU can now write while running, and recover from a power loss without losing changes that were written while engine was running — i.e. long term fuel trims" — @ggurov

### Power-Loss Recovery

The write-while-running design means:
- The SD card always has the latest state (including mid-session tune changes).
- On the next boot, the ECU reads the SD card tune (which has a higher `writeId` than flash) and loads it automatically.
- No changes from the running session are lost even if EFI power is cut abruptly.

### Mega100 Setup for SD Card Logging

To enable this functionality on the Mega100:
- Configure **SPI3** for SD card communication.
- Set the **SD card CS (Chip Select) pin** in the configuration.
- Enable **`SD Card Logging: True`** in the tune settings.
