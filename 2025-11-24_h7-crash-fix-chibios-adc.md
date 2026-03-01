# STM32H7 ECU Crash on Flash Write — ChibiOS ADC Bug Fix

*Source: rusEFI Discord, 2025-11-24 | Channel: 1401895238481744052*
*Contributors: ggurov (@ggurov), mynameisdeleted (@mynameisdeleted), MykkH (@mykkh)*

## Summary

A crash affecting STM32H7-based epicEFI hardware was traced to a condition in the ChibiOS RTOS that could corrupt ADC state during a write-to-flash operation. The fix was implemented as a patch to the epicEFI ChibiOS fork and was included in the 2025-11-24 firmware release.

## Details

**Root cause:**
The ChibiOS core contained a condition that could crash the ECU when writing settings to flash. The bug had reportedly been avoided by coincidence in prior builds but became reliably triggerable on H7 hardware. The crash manifested as an ADC disruption triggered during the flash write sequence.

- Crash condition: write-to-flash path in ChibiOS could disrupt ADC peripheral state on STM32H7.
- Prior behavior: the bug existed but was being avoided by happenstance in earlier firmware builds.
- ggurov characterised it as: *"the core rus has a condition that can crash the ecu on write to flash, and it just so happens to have been getting avoided somehow."*

**Fix:**
The fix required patching ChibiOS directly rather than working around it at the application level. This necessitated forking ChibiOS into an epicEFI-controlled repository so the patch could be applied and maintained independently of upstream.

- Patch commit: `https://github.com/epicEFI/ChibiOS/commit/d3564b007a604ae319072b8d3a7dd9970cc9e5cc`
- Fix documentation (AI-assisted analysis): `https://github.com/epicEFI/epicefi_fw/blob/master/AI/h7_crash.md`
- Repository consequence: epicEFI now maintains its own ChibiOS fork; upstream upgrades will require manual merge work.

**Alternative approach considered:**
A possible application-level workaround was briefly discussed — reinitializing the ADC after a flash write completes — but the ChibiOS patch was shipped as the primary fix.

**Build environment note:**
The debugging work (obtaining a stack trace) was done using WSL2 + Linux ARM toolchain on Windows. ggurov noted he uses WSL2 + Linux `arm-eabi` tools rather than native Windows toolchains (msys2/mingw), finding the Windows PATH configuration too cumbersome. The build scripts contain WSL2-specific assumptions (calling `opencd.exe` rather than a native binary).

**Affected hardware:** STM32H7-based epicEFI ECUs.

**Release:** Fix shipped in the 2025-11-24 firmware build. Confirmed by ggurov in response to user query: builds dated 11-24 and later include the patch.
