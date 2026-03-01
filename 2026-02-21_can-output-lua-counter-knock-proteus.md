# CAN Output Variables, Lua Counters, and Knock Detection on Proteus V3

*Source: epicEFI Discord, 2026-02-21 | Channel: 1439273357424988301*
*Contributors: @ggurov, @atntpt*

## Summary

A user performing a Subaru Legacy EZ30 engine swap (bench testing on a Proteus V3 ECU) asked how to replicate a stock ECU behavior of transmitting a 0-255 incrementing counter at 20 Hz on a CAN bus message. The conversation covered the relationship between Lua scripts and the CAN Outputs feature, how to use `luagauge` and math channels as a workaround for exposing custom variables in CAN output slots, and the use of VIRTUAL_OUTPUT_1 via programmable outputs as an alternative approach. Knock detection on the Proteus V3 running epicEFI was also discussed, with confirmation that a hardware fix had resolved a prior knock issue on that hardware.

## Details

### Context: Subaru Legacy EZ30 Swap, Proteus V3

- Application: Subaru Legacy 07 with an EZ30 engine swap
- ECU: Proteus V3 (STM32F429-based)
- Status at time of conversation: bench-testing CAN communications
- Note: Proteus V3 does not have an SD card slot. The team recommended upgrading to an H7-based board for access to upcoming SD-card-dependent features.

### CAN Outputs vs. Lua for Custom CAN Messages

The user had previously been writing raw CAN messages entirely in Lua. They were exploring the built-in "CAN Outputs" feature as a simpler alternative, but needed to know how to include a custom incrementing counter in that output.

Key finding: The CAN Outputs UI in rusEFI/epicEFI uses internal variable hash IDs to select what data goes into each CAN frame field. As of this conversation, there was no UI mechanism to directly assign a Lua-defined variable (including user counters such as `USER_COUNTER`) as a CAN output field by hash ID. The parsers could potentially be modified to expose Lua hash IDs to CAN outputs, but this was not yet implemented.

### luagauge as a Workaround

`luagauge` was suggested as the primary workaround. A Lua script can write a value into a `luagauge` slot, and that gauge can then be referenced in a CAN output configuration via its hash ID. However, it was noted that `luagauge` slots at the time of this conversation did not have Lua hash IDs assigned, limiting their direct use in the CAN Outputs UI without parser-level changes.

### Math Channel Workaround

An alternative approach: pipe the desired variable into a math channel, then reference the math channel in the CAN bus output configuration. The hash ID `840468510` was mentioned as an example of a math channel hash that can be used in CAN output fields.

Workflow:
1. Define a math channel expression that produces the desired value (e.g., an incrementing 0-255 value).
2. Reference the math channel in the CAN Output field slot using the channel's hash ID.

### VIRTUAL_OUTPUT_1 and Programmable Outputs

For the specific requirement of a 0-255 counter incrementing at 20 Hz (matching stock ECU behavior at approximately every CAN transmit cycle), `VIRTUAL_OUTPUT_1` was suggested as an alternative vehicle. Using the programmable output feature, a condition like `time_toggle_25ms = 1` can be used as the base signal. However, this approach was acknowledged as somewhat "janky" because the current implementation is not truly event-driven per CAN transmit. Improving this behavior (making user counters more event-driven and tightly coupled to the CAN transmit rate) was flagged as a future development item.

### Incrementing Counter Requirements (Stock ECU Replication)

- Counter range: 0x00 to 0xFF (0-255), wrapping on overflow
- Required rate: 20 Hz (matching the stock CAN message transmit rate)
- Missing consecutive counts: likely acceptable for the target application
- The user's previous Lua-only approach (manually constructing and sending CAN frames) had already worked; the exploration was to simplify by using CAN Outputs

### Knock Detection on Proteus V3 with epicEFI

The user reported that knock detection had previously been broken on their Proteus V3 but was fixed by a hardware modification. During the conversation:

- @atntpt confirmed: "Knock is fixed" after applying a hardware fix on the Proteus V3.
- @ggurov requested a bench test to confirm knock detection is working correctly under epicEFI firmware.
- @atntpt agreed to bench test and report back.

The exact nature of the hardware fix was described as "junky but worked on RusEFI." The fix was a hardware-level change to the Proteus V3 board itself.

### SD Card and Future Features

- Proteus V3 does not have an SD card slot.
- The epicEFI team noted that upcoming features will require an SD card. Users planning a hardware upgrade were advised to ensure their next ECU includes an SD card slot (e.g., H7-based boards).
