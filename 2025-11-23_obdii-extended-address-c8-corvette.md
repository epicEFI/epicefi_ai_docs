# OBD-II Extended Addressing: C8 Corvette and Honda-Style PID Replies

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: SanGawku (@sangawku)*

## Summary

The C8 Corvette does not reply to OBD-II PID requests on the standard ISO 15765-4 address (0x7E8). Instead it uses the extended 29-bit CAN address 0x18DAF111, the same extended addressing scheme used by Honda vehicles since 2006.

## Details

### Standard vs Extended OBD-II CAN Addressing

Standard ISO 15765-4 OBD-II uses 11-bit CAN IDs:
- Request: 0x7DF (functional) or 0x7E0–0x7E7 (physical)
- Reply: 0x7E8–0x7EF

Some manufacturers implement OBD-II over 29-bit extended CAN IDs instead, using a J1939-style address format.

### C8 Corvette

The C8 Corvette ECU replies to OBD-II PID requests on CAN ID **0x18DAF111** (29-bit extended frame).

This is the same extended addressing convention used by Honda/Acura vehicles since approximately 2006.

### Implication for epicEFI / rusEFI CAN Sniffing

When attempting to read OBD-II PIDs from a C8 Corvette (e.g., via a Lua `canRxAddMask` listener), the reply filter must target the extended address 0x18DAF111 rather than the standard 11-bit 0x7E8 reply address. Likewise, PID requests must be sent to the corresponding extended request address (0x18DB33F1 for functional addressing in the 29-bit scheme) rather than 0x7DF.
