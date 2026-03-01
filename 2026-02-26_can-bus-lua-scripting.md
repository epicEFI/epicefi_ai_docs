# CAN Bus Message Handling via Lua Scripting in rusEFI

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @VWPat, @ggurov*

## Summary

A user asked how to save and work with CAN bus messages in rusEFI. The answer is that CAN bus message handling (reading, filtering, saving, reacting to) requires writing Lua scripts within rusEFI. The rusEFI wiki has a dedicated Lua Scripting page with full documentation and examples.

## Details

### Requirement

To work with CAN bus messages in rusEFI — whether reading data from the CAN bus, reacting to CAN messages, or logging/saving specific CAN IDs — you must write a **Lua script** inside rusEFI. There is no point-and-click CAN message saving interface; Lua is the required approach.

### Lua Scripting Reference

Full documentation including API reference and examples:

- **rusEFI Lua Scripting Wiki**: https://wiki.rusefi.com/Lua-Scripting/

The wiki covers:
- CAN bus read/write functions
- How to filter by CAN ID
- How to react to messages or log data
- General Lua scripting API for rusEFI
