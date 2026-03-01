# Mega Epic CAN Module: Custom PCB Design and Arduino Library Setup

*Source: rusEFI Discord, 2025-11-22 | Channel: 1434034231713075282*
*Contributors: GGGI (@gunadi), sheckta#1593 (@sheckta_1593), epicEFI admin (@epicEFI_admin), epicEFIdev (@epicefidev)*

## Summary

Community member plans to design a custom PCB for the Mega Epic CAN module with additional protection components, and a separate user encountered a missing library error when attempting to upload the CAN parser code to an Arduino Mega.

## Details

**Custom PCB for Mega Epic CAN module:**
GGGI proposed designing a custom PCB for the Mega Epic CAN module with the following additions:
- Additional **resistors and capacitors** for improved signal integrity and protection.
- The design will be shared publicly with the community once complete.
- The goal is to produce a safer, more robust hardware platform than a bare breadboard or prototype build.

The reference codebase and pinout for the Mega Epic CAN module is at:
- `https://github.com/ggurov/MEGA_EPIC_CANBUS`
- `https://github.com/ggurov/MEGA_EPIC_CANBUS/blob/main/PINOUT.md`

**Arduino Mega CAN code — missing "mnea parser" library error:**
sheckta#1593 reported that uploading the CAN-related Arduino code to an Arduino Mega failed with an error indicating it could not find the "mnea parser" (NMEA parser library).

- Root cause: not all required libraries were installed in the Arduino IDE library manager, even after downloading the full project code.
- Solution: ensure all required libraries are manually installed. The MCP2515 CAN library that comes with the project `.zip` must be added to the Arduino IDE — it is not available through the standard library manager and must be installed from the ZIP file directly.

**Hardware wiring notes for Mega Epic CAN (from earlier channel context):**
- SPI is wired as intended on the Arduino Mega.
- **D2** = interrupt pin for MCP2515.
- **D9** = MCP_CAN chip select (CS).
- The dashboard file (`MEGA_EPIC_CANBUS.dash`) is available from `https://content.epicefi.com/dashboards_publish/`.
- If using a board with an **8 MHz crystal** (instead of 16 MHz), the code must be adjusted for the lower clock speed.
