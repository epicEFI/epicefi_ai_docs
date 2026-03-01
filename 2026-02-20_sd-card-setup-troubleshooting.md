# SD Card Setup and Troubleshooting on epicEFI / Mega100

*Source: rusEFI Discord, 2026-02-20 | Channel: 1401895238481744052*
*Contributors: @MykkH, @BossTheTuga, @ggurov*

## Summary

Multiple users encountered issues getting SD card functionality working on epicEFI hardware (including the Mega100 board). The session covered SD card format requirements, the need to enable SD card logging in firmware settings, a workaround involving an external trigger loop, and a quirk where the system defaults to SD-USB mode on power reset instead of ECU-mount mode. A key finding was that a custom firmware build behaved differently from the "gurov bin" with identical configuration.

## Details

### SD Card Format

@MykkH reported formatting the SD card as **exFAT** and the system failing to recognize it. rusEFI/epicEFI SD card support requires **FAT32** format. exFAT is not supported. Always format the SD card as FAT32 before use.

### Enabling SD Card Logging

The system will not recognize or interact with the SD card unless SD card logging is explicitly enabled in the tune/configuration:

- Setting: **`SD Card Logging: True`**
- @MykkH discovered this was the root cause of their SD card not being recognized: "Ah, had to enable SD card logging"

Note: @MykkH also asked whether enabling this setting would force datalogs to be recorded on the SD card (they wanted SD tune writes without continuous datalogging). This behavior was not fully resolved in the thread but is worth verifying in the specific firmware version being used.

### Trigger Loop Workaround for SD and Flash Issues

@MykkH found that enabling an external trigger on an **unused output** and **looping it back to the trigger input** resolved issues with both SD card and flash memory operating as expected. This is described as getting "SD and flash to act as expected."

Configuration:
1. Enable external trigger on an unused output pin.
2. Wire (loop) that output pin back to the trigger input.
3. Both SD write and flash operations then work correctly.

This suggests the system may require trigger activity to enable certain SD/flash write operations, or that the trigger loop satisfies an internal readiness condition.

### SD USB vs. Mount to ECU on Boot

On power reset, the system defaults to **SD USB mode** rather than **ECU mount mode**, which prevents `sdWrite` from functioning automatically.

Symptom: After a power cycle, SD card writes do not happen until the user manually:
1. Clicks **Unmount**
2. Clicks **Mount to ECU**

@MykkH asked whether this can be automated on boot. No automatic solution was confirmed in this session — this may require a firmware modification or startup script change.

### Custom Bin vs. Gurov Bin Behavior Difference

@BossTheTuga reported that a **custom firmware build** had SD card functionality working correctly, but the same hardware configuration on the **"gurov bin"** (a specific pre-built firmware binary) did not work. This implies there may be a board-specific build or configuration flag difference between the two binaries that affects SD card initialization or SPI assignment.

Both @MykkH and @BossTheTuga noted this appeared to be a broader firmware issue rather than individual board misconfiguration.

### Mega100 SD Card Setup Requirements

For the Mega100 board specifically, SD card functionality requires:
- **SPI3** must be configured for SD card communication
- The **SD card Chip Select (CS) pin** must be configured

> "@Aj: yes applicable to mega100, you'll need to set up SPI3 + sdcard CS" — @ggurov
