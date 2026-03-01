# Troubleshooting Online Flasher: Manually Entering DFU Mode

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @DCwerx*

## Summary

The rusEFI/epicEFI online flasher can sometimes be unreliable and fail to detect or flash the board. A reliable workaround is to manually force the board into DFU (Device Firmware Update) mode before connecting USB, bypassing the flasher's automatic detection step.

## Details

### Symptoms

- Online flasher fails to detect the board or complete the firmware flash.
- The flasher appears "picky" and does not reliably put the board into DFU mode on its own.

### Workaround: Manual DFU Mode

1. Locate the **boot button** on the rusEFI/epicEFI board.
2. **Hold the boot button down** while connecting the USB cable to the PC.
3. Keep holding the button until the USB connection is established.
4. The board will now be in DFU mode and the online flasher (or a DFU utility) should be able to flash it.

### Notes

- This method works around timing or driver issues that can prevent the flasher from automatically asserting DFU mode.
- If the board still does not appear, try a different USB cable or USB port, then retry the manual DFU procedure.
