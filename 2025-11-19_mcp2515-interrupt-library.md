# MCP2515 CAN Controller: Interrupt Library Requirement
*Source: rusEFI Discord, 2025-11-19 | Channel: 1434034231713075282*
*Contributors: @ggurov (ggurov)*

## Summary

When using the MCP2515 CAN bus controller module, interrupt support requires adding a specific library that is bundled with the module's .zip package. The standard library does not include interrupt support — it must be manually added from the included zip file.

## Details

### Issue

The default MCP2515 library does not support hardware interrupt-driven operation. Without interrupt support, the driver must rely on polling, which is less efficient and may miss messages at higher bus loads.

### Solution

The MCP2515 module (or its associated software distribution) comes with a `.zip` file containing an additional library that adds interrupt support. This library must be:
1. Extracted from the accompanying `.zip` file.
2. Added to the project's library path (e.g., Arduino libraries folder, or the relevant include path for the firmware build system).

Once the interrupt-capable library is installed, the MCP2515 can be configured to assert an interrupt line when CAN frames are received, allowing the firmware to handle CAN messages via interrupt service routines rather than polling.

### Relevance

This is relevant for any rusEFI/epicEFI build that uses an external MCP2515 CAN controller (common on RP2040-based or other microcontroller boards that lack a native CAN peripheral). Interrupt-driven CAN is important for real-time performance in engine management applications.
