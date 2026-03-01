# uaefi/Proteus Recovery from Cylinder Count Mismatch Crash

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov, @offtheband*

## Summary

A known failure mode exists in uaefi (and Proteus) where configuring a mismatched number of cylinders causes the ECU firmware to crash and enter an unrecoverable state via normal means. This issue affects multiple users and hardware variants. Recovery requires using STM32CubeProgrammer rather than normal firmware flash methods.

## Details

### Failure Mode

- Cause: Setting the cylinder count in firmware to a value that mismatches the actual hardware/engine configuration (e.g., changing from 4 to 8 cylinders or vice versa without a clean flash)
- Symptom: ECU crashes on boot and becomes unreachable via USB/serial — normal reflash via TunerStudio or epicTuner fails
- Affected hardware: uaefi ("super uaefi" mentioned specifically), Proteus
- The issue has been reported by multiple users who did not previously report it — likely underreported

### Recovery Method

Use **STM32CubeProgrammer** to perform a low-level flash:
1. Connect ECU via USB DFU mode or ST-LINK
2. Use STM32CubeProgrammer to erase flash and write fresh firmware binary
3. This bypasses the crashed bootloader state that prevents normal flashing

### Notes

- The same crash behavior was reported in the main rusEFI community as well (not epicEFI-specific)
- Community suggestion: document this as an official recovery procedure since it affects multiple hardware variants
- offtheband confirmed they experienced this on their super-uaefi and found the CubeProgrammer solution referenced in a comment thread

### Related: super-uaefi vs uaefi121 Hardware Notes (from same conversation)

- super-uaefi: STM32 F7 processor
- uaefi121: STM32 F4 processor
- Proteus: originally F4, F7 variant now available from rusEFI shop
- "huge" board: positioned above Proteus in the lineup (processor unconfirmed in this thread)
- Repurposing unused injector outputs as general digital I/O is possible on super-uaefi and was noted as a useful workaround for expanding I/O
- Pins 23-34 on super-uaefi are all sensor ground — users noted this is more sensor grounds than typical applications need, and some could have been general-purpose I/O instead
