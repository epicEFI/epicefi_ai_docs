# CAN Bus Sniffing and BCM Security Bypass with Raspberry Pi

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @ToyotaTerrorist, @Lysdex*

## Summary

A discussion covering how to use a Raspberry Pi with a USB-to-CAN adapter to sniff CAN bus traffic for the purpose of reverse-engineering BCM (Body Control Module) security authentication on a Ford Falcon. The stock BCM requires a specific CAN handshake on power-up to unlock the starter circuit and radio. The recommended approach is to prototype the sniffing and replay logic in shell/Python first, then port the working solution to ECU Lua scripting. The rusEFI firmware's built-in `set verbosecan on` console command can also be used to dump received CAN traffic without external hardware.

## Details

### Background: Ford Falcon BCM CAN Security

ToyotaTerrorist needed to communicate with the stock BCM on a Ford Falcon. The BCM performs a single CAN-based authentication check on power-up; once the check passes it unlocks the starter circuit and radio, and the ECU can then be disconnected for the rest of the key cycle. The exact authentication payload is not publicly documented — Haltech implements a "Ford CANbus mode" that handles this transparently in their software, but they do not expose the underlying CAN data.

Key behaviour noted:
- The BCM check happens once at power-up, not continuously.
- After the BCM accepts the handshake, the ECU can be disconnected and the BCM remains unlocked until the key is switched off.
- The OBD-II port exposes the CAN bus and can be used to flash/modify all systems.
- There may be a public/private CAN bus separation to be aware of on some vehicles.

### Recommended Sniffing Approach

@ggurov recommended:
1. Hook up a Raspberry Pi to the car's CAN bus using a USB-to-CAN adapter.
2. Capture the CAN traffic from the stock ECU during power-up using standard Linux `can-utils` tools.
3. Write and test replay logic in shell/Python on the RPi (avoids slow firmware recompile cycles).
4. Once proven, translate the working code to ECU firmware or Lua scripting.

> "yeah, but you don't know how, and recompiling shit all the time is stupid — get it working, then translate that to ecu/lua/etc"

Reference project for CAN button box / CAN interaction code: https://github.com/ggurov/canButtonBox

### USB-to-CAN Adapters for Raspberry Pi

The Raspberry Pi does not have a native CAN interface; a USB-to-CAN (SocketCAN) adapter is required.

Compatible adapters confirmed to work:
- **canable** — inexpensive, widely available, SocketCAN compatible
- **pcan** — PEAK-System adapter, also SocketCAN compatible
- Most USB SocketCAN adapters work on the RPi

Example AliExpress adapter: https://www.aliexpress.us/item/3256807909892182.html
Example Amazon adapter: https://www.amazon.com/Converter-transceiver-debugger-Protocol-socketcan/dp/B0DBZGWCC8/

Note: ToyotaTerrorist found US-based links would not ship to Australia; local equivalents should be sourced.

### Sending CAN Messages from Shell (can-utils)

The `cansend` command from `can-utils` is used to transmit CAN frames:

```bash
cansend can0 710#020005
cansend can0 710#$PAYLOAD
cansend can0 710#04$CRC32
```

Where:
- `can0` is the SocketCAN network interface
- `710` is the CAN arbitration ID (example from the Falcon BCM sequence)
- The payload structure follows a three-frame pattern: initiation, payload body, and CRC32 verification

There is also a Python `can` module available for more complex scripting and debugging.

### Sniffing CAN Traffic via rusEFI Console

If a USB-to-CAN adapter is not available, rusEFI itself can be used as a CAN sniffer by powering it over USB and enabling verbose CAN logging from the rusEFI console:

```
set verbosecan on
```

or alternatively:

```
set verbose_can on
```

This causes the ECU to dump all received CAN frames to the console output.

Procedure:
1. Power the rusEFI ECU over USB (laptop or wall plug power — no car 12V needed).
2. Connect the ECU's CAN bus wiring to the vehicle's CAN bus (or OBD-II port).
3. Enable verbose CAN in the console.
4. Observe received frames during BCM power-up sequence.

### OBD-II Port and CAN Bus Access

OBD-II ports typically include the vehicle CAN bus on pins 6 (CAN-H) and 14 (CAN-L). This provides a convenient connection point for a sniffer without needing to tap wiring harnesses directly.

### Notes on Proprietary CAN Protocols

- Haltech has Ford CANbus mode built in but does not expose the underlying CAN message data to users.
- The PCMHacking forums contain community research on Ford CAN protocols:
  - https://pcmhacking.net/forums/viewtopic.php?t=8542
  - https://pcmhacking.net/forums/viewtopic.php?t=8424
- @Lysdex noted it is possible (but not guaranteed) that someone in the community has already published the relevant CAN data from independent reverse-engineering efforts.
