# Staged Injection Bug Fix: Testing on Proteus F4 and TeraFlash Firmware Flashing

*Source: epicEFI Discord, 2026-02-22 | Channel: 1401895238481744052*
*Contributors: @tmbryhn (@tmbryhn), @ggurov (@ggurov)*

## Summary

A staged injection bug fix was built into a daily epicEFI firmware build on 2026-02-22. ggurov verified the fix with a logic analyzer and built firmware for all variants. tmbryhn volunteered to independently verify on a Proteus F4 board. During testing, tmbryhn reported that the fix was not fully effective: the system behaved correctly up to 92% staging in TunerStudio, but at 93% staging the primary injector report dropped to 0 ms and all 8 outputs became erratic, and at 100% staging all outputs turned off completely. The flashing process used TeraFlash (web-based tool) since the console flash method was no longer working as expected with epicEFI firmware.

## Details

### Staged Injection Bug: Symptoms

tmbryhn's test results on Proteus F4 with the patched daily build:

- **0% to 92% staging:** Primary and secondary injector pulse widths report correctly as "x.xx msec" in TunerStudio
- **At 93% staging:** Primary injector report drops to 0 ms; all 8 injector outputs become erratic
- **At 100% staging:** All 8 outputs turn completely off
- Behavior is reproducible
- A log file was captured during testing

The fix was considered incomplete as of this test; ggurov acknowledged the issue and planned to investigate further.

### Build and Verification Process

- ggurov built firmware for all variants on 2026-02-22
- Initial verification was done by ggurov on a logic analyzer before releasing builds
- Independent hardware verification by community members was requested before a formal new release
- tmbryhn tested on: **Proteus F4** variant

### Firmware Flashing: TeraFlash vs Console Method

tmbryhn reported that the traditional **console flash method** no longer works reliably with epicEFI firmware (as of this date). The recommended alternatives:

1. **TeraFlash** (web-based flash tool) — currently the primary working method
   - URL: `https://content.epicefi.com/teraflash2/`
   - Note: the epicEFI homepage firmware page (`https://content.epicefi.com/firmware/`) links to an "online flash tool" but this may still point to the legacy flasher
   - TeraFlash 2.0 was described as having a "cyberpunk" UI style

2. **STM32CubeProgrammer** — recommended by ggurov as the most straightforward flashing method for STM32-based boards

### Finding the Correct Daily Build

When testing a specific daily build (not the latest release), the build must be downloaded directly from the link provided by the developer. Clicking "latest" on the firmware page will not include unreleased daily builds. tmbryhn initially retested with a build he thought was patched but was still running an unpatched version — confirmed by checking the firmware signature/date.

### SD Card Reliability Note

A brief observation was made during the same session: the SD card functionality on epicEFI hardware was described as "sketchy" (unreliable). This was in the context of a user (Aboy) attempting to get the SD card tune backup feature working on their f4/f7 board.

### F4/F7 Board: SD Card Tune Backup Feature

For f4 and f7 class boards, epicEFI supports:
- **Write-while-running:** tune can be saved to flash while the engine is running
- **SD card tune backup:** tune is automatically backed up to an SD card

This significantly reduces the risk of losing a tune during a power cycle or flash corruption event. Users experiencing SD card issues should resolve the SD card functionality before relying on this backup path.
