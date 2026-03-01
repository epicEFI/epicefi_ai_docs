# SD Card Tune Backup: Validation Procedure

*Source: rusEFI Discord, 2026-02-26 | Channel: 1439273357424988301 (dev/testing)*
*Contributors: @ggurov, @tmbryhn*

## Summary

When validating SD card tune backup functionality on rusEFI/epicEFI hardware, the firmware must survive both hard power-disconnect and soft-reboot events without data loss, and must boot from the SD card on the next power-up. @tmbryhn confirmed the feature passes all required test conditions on a Proteus F4 bench setup.

## Details

### Required Test Conditions

Per @ggurov's validation checklist, SD card tune backup must be verified under the following scenarios:

1. **Write while running stim** — write the tune to SD card while the engine simulator is active
2. **Hard power disconnect** — cut power while running; verify on next boot that:
   - No tune data is lost
   - The firmware displays / logs `"boot from SDCARD"` on startup
3. **Soft reboot** — trigger a firmware reset (soft-reboot) while the stim is running; verify same data integrity and boot message
4. **RPM = 0 case** — confirm the feature also works correctly when engine RPM is zero (i.e., key-on but not cranking/running)

### Test Result (2026-02-26)

@tmbryhn confirmed on a Proteus F4 with on-board SD card:

- All four scenarios passed
- "Works like a charm, both during power disconnect and soft-reboot"
- "Behaves fine when rpm=0 as well as in running condition"

### Hardware Used

- Proteus F4 ECU
- On-board SD card slot populated
- 2-channel knock input installed
- Bench stim (engine simulator) connected

## Notes

- The `"boot from SDCARD"` boot message is the key indicator that the firmware correctly detected and loaded the tune from the card rather than internal flash
- This validation is recommended after any MCU swap or firmware reflash on SD-card-equipped hardware
