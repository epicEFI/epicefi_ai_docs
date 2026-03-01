# SD Card Behavior and STM32 Flash Write Protection in epicEFI

*Source: rusEFI Discord, 2026-02-21 | Channel: 1356732771325968630*
*Contributors: @jeybee, @ggurov, @Robb235*

## Summary

Discussion covered how the epicEFI firmware interacts with SD cards (partition handling, tune storage, engine-safe operation) and how flash writes to the STM32 are managed to avoid hitting the flash endurance limit. The firmware uses a write-ID system to prevent redundant flash writes, and the SD card acts as a tune storage/restore mechanism without being required for engine operation.

## Details

### SD Card Partition Behavior

Testing by `jeybee` revealed:

- The firmware **does not partition the SD card itself** — it only formats existing partitions.
- The firmware **uses the last partition on the card**, whatever is already there.
- `ggurov` added: **"it does SOMETHING, but it certainly doesn't bulldoze the card"** — the firmware preserves existing partition data.
- A future improvement discussed: adding an explicit **"initialize card"** function to clean-format a card from within the firmware.

### SD Card as Tune Storage (Not Required for Operation)

- The SD card is **not a hard requirement** — the engine will still run without one.
- If an SD card is present and the engine is not running at boot: the firmware **reads the tune from SD card and updates flash**.
- If an SD card is removed or loses contact while the engine is running: **the engine will not die**.
- Primary use case: writing tunes while the engine is running and having them **restore on power loss**.
- The existing flash-based tune storage method continues to work as before.
- `jeybee` expressed concern about the SD card holder physically opening on a hard bump — confirmed that losing the card mid-run is safe.

### STM32 Flash Write Endurance

- STM32 flash endurance: approximately **10,000 write cycles** per page.
- Concern: hitting the flash write limit during development or frequent tuning sessions.
- `ggurov`'s response: the SD card helps reduce direct flash writes.
- **Write-ID protection mechanism**: a write to flash only occurs if the incoming **writeId is higher than the current flash writeId** — every write increments the writeId, preventing duplicate or unnecessary writes.
- This prevents runaway write loops from wearing out flash.
- Example failure scenario discussed: if somehow stuck in an erase loop at once per second, the STM32 flash would be exhausted in approximately **3 hours**.
- No built-in counter exists to track total flash write cycles (as asked by Robb235) — the write-ID mechanism is the primary safeguard.

### SD Card Recommendations

- `jeybee` asked about future SD card requirements. Current stance (ggurov):
  - No specific capacity requirement enforced.
  - Card is optional for operation.
  - Any card with compatible partitioning will work; the firmware uses the last partition.
