# F4/F7 Flash Write Behavior: "Burn" Button, SD Card, and Tune Persistence

*Source: epicEFI Discord, 2026-02-27 | Channel: 1356732771325968630*
*Contributors: @ggurov, @Detonation, @Aj, @jeybee*

## Summary

On STM32 F4 and F7 boards, the ECU cannot write to internal flash while the engine is running because the flash erase operation blocks execution. The "Burn" button in TunerStudio does NOT write to flash during runtime on these targets. Settings and table changes are applied immediately in RAM and take effect, but they are not persisted to flash without an SD card. An SD card is strongly recommended: with one present, "Burn" writes to the SD card with an incrementing write-ID, and flash is synchronized when the engine is shut down.

## Details

### Core Limitation: F4/F7 Cannot Write Flash While Running

- STM32 F4 and F7 processors use flash that requires an **erase operation before writing**.
- Erasing flash blocks CPU execution — this is incompatible with real-time engine management.
- Therefore, on F4/F7 targets: **"Burn" does not write to internal flash while the engine is running**.
- This is a hardware-level constraint, not a software bug.

### What "Burn" Actually Does

| Condition | What Burn Does |
|---|---|
| No SD card | Changes live in RAM only. Values take effect immediately but are lost on power-off. |
| SD card present | Writes tune to SD card with an incrementing write-ID. Flash is written when engine stops. |

- Settings are **always applied to RAM immediately** — changes are active as soon as they are made, regardless of SD card presence.
- To save without an SD card: park the vehicle, shut the engine off, and let the ECU write to flash before removing power. Do not "park and yank" — power must remain until the write completes.

### SD Card Write Flow

1. User presses "Burn" → tune written to SD card with write-ID N.
2. Engine is running → flash is NOT updated yet.
3. Engine shuts down → ECU compares write-ID on flash vs. SD card.
4. If SD card has higher write-ID → ECU writes SD card tune to flash.
5. On next boot → ECU selects the highest write-ID source (flash or SD card) to boot from.
6. If power is cut before flash write completes → ECU boots from SD card on next power-on (safe fallback).

### USB Power and Write-on-Shutdown

- When the vehicle ignition is cut, main ECU power drops and the engine stops.
- If a USB cable is still connected, USB continues to power the ECU after ignition-off.
- The ECU uses this USB-powered window to write the tune from SD card to flash.
- This is the intended write mechanism for vehicles where ECU power is tied to ignition.

### CAN Bus Tuning

- Tune writes over CAN are possible via a CAN-to-USB bridge (TunerStudio or EFI Tuner can use this bridge).
- If power is cut before the write completes, memory is lost — writes over CAN do not persist differently than any other write path.
- The same SD card / flash write-ID mechanism applies regardless of how the tune is delivered.

### Recommendations

- **Use an SD card** on any F4/F7-based board (uaEFI, etc.) for reliable tune persistence.
- SD card also enables logging and other runtime tracking features.
- If no SD card is used: always allow a clean engine-off → power-off sequence to give the ECU time to write flash.
- The W25 (external SPI flash chip) is not necessary if SD card support is available; the SD card replaces that role.

### Hardware Context

- Confirmed affected targets: F4-based and F7-based boards.
- H7-based boards may have different flash write characteristics (not covered in this thread).
