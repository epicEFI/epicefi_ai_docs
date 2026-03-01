# GPS Integration via CAN Bus on Arduino Mega with epicEFI

*Source: rusEFI Discord, 2025-11-21 | Channel: MEGA_EPIC_CANBUS dev (1434034231713075282)*

*Contributors: ggurov, Aboy (aboy0534_42329)*

## Summary

The MEGA_EPIC_CANBUS project added GPS input support to the Arduino Mega CAN bus companion board. GPS data (speed, coordinates, altitude, heading, date/time) is parsed from NMEA serial output and forwarded over CAN to the epicEFI ECU. The recommended GPS module is the ublox GT-U7 or GT-U8, configured via u-center software. The STM32H7-based ECU board was also confirmed working with CAN bus.

## Details

### GPS Hardware: GT-U7 vs GT-U8

Two generations of ublox-based GPS modules were evaluated:

| Module | Update Rate | Notes |
|--------|-------------|-------|
| GT-U7  | 10–20 Hz    | Confirmed working; ggurov has units on hand |
| GT-U8  | Higher (gen) | Recommended if locally available; should be compatible |
| GT-U6  | Lower        | Older generation, also available |

- The GT-U7 update rate is **10–20 Hz**
- The GT-U8 is a newer ublox generation and is expected to perform better; ggurov confirmed it should be compatible
- Both modules output **NMEA sentences** over serial UART
- Module configuration (update rate, baud rate) is done through **ublox u-center** software

Recommendation: purchase GT-U8 if available locally; GT-U7 is acceptable. Avoid modules that appear to be counterfeit (low-quality PCB, no proper markings).

### GPS Serial Interface Configuration

The GPS module connects to the Arduino Mega via UART:

- **Pins:** TX2 / RX2 (Arduino Mega hardware UART 2)
- **Baud rate:** 115,200 bps

Code repository: https://github.com/ggurov/MEGA_EPIC_CANBUS

The firmware parses NMEA sentences from any compatible serial GPS receiver. Because only standard NMEA parsing is used, any serial GPS module should work — not just ublox modules.

### Data Logged from GPS

The following data fields are captured from the GPS module and forwarded:

- Speed
- Latitude
- Longitude
- Altitude
- Course (heading)
- Date and time

Planned future features:
- **RTC synchronization**: automatically set the real-time clock from GPS satellite time, with a configurable GMT offset
- **%split calculation**: compare GPS-derived speed against VSS (vehicle speed sensor) data
- **Track mapping**: positional data overlaid on engine data logs to enable comparing runs on the same section of road ("WOT pulls on that one road")

### CAN Bus Hardware Validation

Aboy confirmed that the **STM32H7 MCU board** successfully communicates over CAN bus. Testing of the Arduino Mega CAN bus path was the next step in the development sequence.

The project is structured as a two-board system:
1. Arduino Mega — handles GPS parsing, peripheral I/O, forwards data over CAN
2. epicEFI ECU (STM32H7-based) — receives CAN messages from the Mega

### Firmware Availability

The u8 firmware build (corresponding to the GT-U8 GPS module or an internal firmware designation) was confirmed as locally available and is the version to use for current development work.
