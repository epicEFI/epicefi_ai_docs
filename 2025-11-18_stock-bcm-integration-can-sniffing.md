# Integrating a Stock BCM with rusEFI/epicEFI via CAN Bus Sniffing

*Source: rusEFI Discord, 2025-11-18 | Channel: rusEFI-builds (1356732771325968630)*
*Contributors: @toyotaterrorist, @ggurov*

## Summary

A user attempting to retain their stock Body Control Module (BCM) alongside a rusEFI/epicEFI swap was advised to first sniff the OEM CAN bus using a Raspberry Pi, understand the factory BCM message traffic, and then reproduce or respond to the required messages from the ECU using Lua scripting.

## Details

### The Problem

When swapping to rusEFI/epicEFI while keeping the stock BCM, the BCM expects to see certain CAN bus messages from the engine management system (RPM, coolant temp, VSS, etc.). If those messages are absent or malformed, the BCM may generate fault codes, disable features, or behave unexpectedly.

### Recommended Approach

1. **Sniff the OEM CAN bus first.**
   Use a Raspberry Pi with a CAN bus sniffer interface (e.g., MCP2515 hat or USB CAN adapter) to capture the raw CAN traffic while the stock ECU is still running. This reveals what message IDs and data formats the BCM expects.

   > "hook up a raspberry pi to it with a sanbus sniffer"

2. **Get the original system working.**
   Before attempting any translation, ensure the car runs correctly with the stock ECU so you have a valid baseline of CAN traffic to capture and analyze.

3. **Translate to rusEFI/Lua.**
   Once the required CAN messages are identified, implement them in the rusEFI Lua scripting environment (or via the CAN bus output configuration in TunerStudio) to reproduce the signals the BCM needs.

   > "get it working, then translate that to ecu/lua/etc"

### Tools

- **Raspberry Pi** with a CAN bus sniffer HAT or USB-to-CAN adapter
- **candump / socketcan** (Linux CAN tools) for capturing and logging messages
- **rusEFI Lua scripting** for transmitting custom CAN frames from the ECU
- **TunerStudio CAN configuration** for standard broadcast messages (RPM, coolant temp, etc.)
