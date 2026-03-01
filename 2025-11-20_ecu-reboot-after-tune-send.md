# ECU Reboot Required After Sending Tune for Full Re-initialization

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, Tera (teraflopping)*

## Summary

After sending a new tune (configuration) to an epicEFI ECU via TunerStudio, some subsystems may not be fully re-initialized at runtime. A power cycle or ECU reboot is sometimes required to ensure all settings take effect cleanly. This is a known behavior, not necessarily a bug.

## Details

- When settings are changed and burned to the ECU, the firmware attempts to apply changes live. However, certain initialization paths (particularly those related to sensor configuration, CAN setup, or output channel configuration) only run fully on a cold boot.
- **Symptom**: After sending a tune, a sensor or output behaves unexpectedly or shows stale values despite the configuration change.
- **Resolution**: Reboot the ECU (power cycle the ignition, or use the "Reboot ECU" option in TunerStudio).
- ggurov: "sometimes when you send the tune, things don't get fully reinitialized runtime — and need to start over."
- Tera confirmed this resolved an issue with the second CAN UEGO bank not showing correctly until a reboot.

## DFU Mode Reboot

- A separate reboot option exists to reboot directly into **DFU (Device Firmware Update) mode**, accessible from TunerStudio.
- This is used for firmware flashing via the web UI or USB DFU tool.
- To use the web UI for flashing: ensure the USB driver is installed, then enter DFU mode (either via TunerStudio's "Reboot to DFU" command or the hardware button). The web UI then handles the rest.
