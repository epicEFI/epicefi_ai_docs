# u-blox GPS Module Initial Setup: Baud Rate and NMEA Configuration

*Source: rusEFI Discord, 2025-11-25 | Channel: 1434034231713075282*
*Contributors: ggurov (@ggurov), Aboy (@aboy0534_42329)*

## Summary

Integrating a u-blox GPS module with epicEFI requires a one-time initial configuration step using the u-blox u-center software over a direct serial connection (via FTDI or similar USB-serial adapter) to set the baud rate to 115200 and increase the NMEA refresh rate before connecting to the ECU's TX2/RX2 UART port.

## Details

**Target connection on epicEFI board:**
The GPS module connects to the ECU's `TX2 / RX2` UART port.

**Problem with out-of-box u-blox modules:**
u-blox GPS modules ship with a default baud rate (commonly 9600 baud) that is too slow for the higher refresh rates useful for vehicle speed/position applications. The ECU expects a specific baud rate, so the module must be pre-configured before wiring it to the ECU.

**Required tool:**
- **u-blox u-center** — the official u-blox configuration and evaluation software. Available from the u-blox website. Must be installed on a PC.

**Initial setup procedure:**
1. Connect the GPS module directly to a PC using an **FTDI** (or equivalent USB-serial adapter), not through the ECU.
2. Open u-blox u-center and connect to the module's COM port.
3. Verify the module is outputting NMEA sentences (visible in u-center's serial console / text view).
4. Navigate to the baud rate configuration and set it to **115200 baud**.
5. Increase the NMEA message refresh rate as required (u-center allows setting the measurement rate independently of baud rate).
6. Save the configuration to the module's non-volatile memory (CFG-SAVE command in u-center).
7. Disconnect from the PC and wire the module to the ECU's TX2/RX2.

> "Actually, you need to connect to it with serial first and set baudrate" — ggurov
> "You need u-blox u-center software and connect to the module using FTDI or something and configure it inside to be 115200, and higher refresh rate" — ggurov
> "Well, connect to them first, see if they spit out NMEA messages" — ggurov

**Summary of key settings to configure in u-center:**

| Setting | Value |
|---------|-------|
| Baud rate | 115200 |
| NMEA refresh rate | Higher than default (application-dependent) |
| Save to NVM | Required (otherwise settings reset on power cycle) |
