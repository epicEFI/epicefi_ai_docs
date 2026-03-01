# SD Card Tune Storage, WriteId System, and Boot Tune Selection

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @ggurov, @MykkH*

## Summary

rusEFI/epicEFI uses a `writeId`-based system to manage which tune is loaded at boot. Tunes are stored both in flash memory and on the SD card; the ECU always picks the valid tune with the highest `writeId` at startup. When real-time changes are made while the engine is running, they are written first to the SD card, allowing recovery if power is lost unexpectedly. After a clean engine-off cycle, any out-of-date flash storage is synchronized with the tune that was booted.

## Details

### Tune File Format

SD Tune Store files are saved as `.xml` files on the SD card (confirmed by @tuga702).

### WriteId-Based Tune Selection

- On boot, the ECU scans both flash memory and the SD card for valid tunes.
- The tune with the **highest `writeId`** is selected as the active tune.
- This ensures the most recently written tune (whether from a tuning session or a firmware-pushed update) is always used.

### SD Card Write on Power-Off

- When the engine is running, real-time tuning changes (e.g., long-term fuel trim corrections) are written to the SD card first.
- The SD card write creates a tune with a `writeId` higher than what is currently stored in flash.
- On the next boot, the ECU picks up that SD-card tune due to its higher `writeId`.

Example explanation from @ggurov:
> "it will write sd tune, if that succeeds, it has written a tune with a higher writeId than what's in flash"
> "on boot, it picks a valid tune with the highest writeId"

### Flash Synchronization After Clean Shutdown

- If the engine boots, then shuts off cleanly, and the flash copy is outdated, the ECU **syncs the flash** with the tune it booted from.
- This keeps flash and SD card in agreement after normal operation cycles.

> "but if it boots, engine off, and flash is out of date, it syncs it with what it booted from" — @ggurov

### Power-Off Timing Concern

@MykkH raised the question of whether a delay between engine-off and ECU power-cut is needed to allow SD writes to complete. The system's internal design (writing to SD first during running) means the SD already has the latest state before flash sync is attempted post-shutdown — so the critical data is preserved even on an abrupt power cut.

### bootFlash and bootSD Priority Values

@ggurov confirmed the following priority settings for the boot source:
- `bootFlash = 9`
- `bootSD = 10`

SD has a higher priority value (10 > 9), meaning SD-stored tunes take precedence over flash when `writeId` values are equal or when explicitly configured this way.

### Rationale for Storing Two Tunes in Both Flash and SD

@MykkH asked why two tunes are stored in each location (flash and SD). This redundancy provides:
- A fallback tune if the primary tune becomes corrupted
- Flexibility to switch between tunes (e.g., a base tune and an aggressive tune) without reflashing
- The `writeId` system arbitrates which of the four possible tunes (2 flash + 2 SD) is actually used at boot
