# SD Card vs Flash Memory: Tune Storage and Write ID Priority in epicEFI/rusEFI

*Source: epicEFI Discord, 2026-02-27 | Channel: 1356732771325968630*
*Contributors: @ggurov, @jeybee, @Detonation, @Aj*

## Summary

epicEFI/rusEFI uses a write ID system to arbitrate which copy of the tune takes effect at boot — the copy with the higher write ID wins, whether it resides on an SD card or in on-chip flash. Without an SD card the ECU runs normally but settings are not persisted to flash while the engine is running; they are committed to flash only after the engine stops. With an SD card present, writes happen during operation, and the SD card always holds the most recently "burned" tune (higher write ID), so it takes precedence over flash on the next boot. After the engine stops, the ECU synchronizes the SD card tune down to flash, equalizing the write IDs.

## Details

### Operation Without an SD Card

- The ECU boots and runs normally without an SD card.
- Tune changes (VE table edits, idle PW, settings, etc.) take effect immediately in RAM.
- Changes are **not** written to flash while the engine is running.
- To persist a tune change without an SD card: **park** (stop editing in TunerStudio), shut the engine down, and allow the ECU to finish writing to flash before removing power.
- This behavior has existed since the earliest rusEFI firmware; it is not a regression.

### Operation With an SD Card

- When a "burn" command is issued (from TunerStudio or over CAN bus), the tune is written to the SD card with a **higher write ID** than the current flash copy.
- On the next ECU boot, the firmware reads the write ID from both the SD card and flash. The copy with the higher write ID is loaded as the active tune.
- SD card tune files are binary files stored in the filesystem: `tune_primary.bin` and `tune_backup.bin`. They are not raw sector writes — they are proper files.
- The tune bytes are essentially an identical copy of what would be written to flash, formatted for the specific board and firmware version.

### Flash Synchronization Logic

- After loading a tune from the SD card, the ECU checks whether the engine is spinning (crank input / RPM present).
- **If the engine is not spinning:** the ECU writes the SD card tune to flash so both copies are synchronized (same write ID). Subsequent boots will use flash directly.
- **If the engine is spinning:** the ECU defers the flash write and continues running from the SD card tune. Flash is written after the engine stops.
- The firmware always tries to keep flash and SD card in sync, but defers it until it is safe to do so (engine not spinning).

### Boot Priority and Race Condition

- If the SD card tune has a higher write ID than flash at power-up, the SD card tune wins.
- Synchronization to flash occurs during initialization, **before cranking begins**.
- If the engine is commanded to crank immediately after power-up (before sync completes), there is a **race condition**: the flash write may not finish before cranking starts. In that scenario the ECU still runs from the SD card tune, but flash may remain out of date until the next engine-off cycle.
- Demonstrated behavior (bench test on F4 hardware): pressing the physical reset button while the engine is running causes an immediate reboot. The ECU picks up the SD card tune (in the test, write ID 782), and the engine continues running without interruption. After stopping the engine, flash is updated and subsequent boots use flash.

### Updating a Tune via SD Card File Copy

- You cannot simply copy an arbitrary `.bin` file onto the SD card and expect it to load. The tune binary is tightly coupled to the board type and firmware version, and carries an embedded write ID.
- Updating a tune is done through the normal burn workflow (TunerStudio → Burn, or CAN bus → TS/ET bridge), which correctly stamps the write ID and writes to the SD card.

### Tune Changes Are Live Without Reboot

- VE table and most tuning parameter changes are applied **immediately in RAM** — no reset or reboot is required to feel the effect.
- "Burn" is purely a persistence operation (saves the current RAM state to SD card / flash so it survives a power cycle).
- An optional "ECU Read Only" mode locks the ECU against in-session modifications.

### CAN Bus Burn Workflow

- Tune burns can be sent over CAN bus using a TunerStudio (TS) or EcuTool (ET) bridge.
- When a burn arrives over CAN, it is stored to the SD card (if present) with a higher write ID, and will take effect on the next boot.
- If power is cut immediately after a CAN burn without giving the ECU time to complete the write, the write is lost — the ECU must remain powered until the write completes.

### SPI EEPROM / Flash as SD Card Replacement

- There is some existing support in the codebase for SPI EEPROM/flash chips, but no hardware with this configuration is actively used by the core developers.
- SPI EEPROM/flash is unlikely to replace SD cards in future designs unless the longer-term storage requirements (data logging, etc.) change significantly. SD cards are preferred because they are also used for data logging.
- If a board's W25 (SPI flash) IC is not needed, the SPI1 pins can be repurposed for GPIO.
