# CAN Bus ADC Expander Using Arduino Mega2560

*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov, @mitch0s*

## Summary

@ggurov announced adding 16 additional analog input (ADC) channels to rusEFI/epicEFI via CAN bus using an Arduino Mega2560 as the CAN expander node. @mitch0s responded with interest in the architecture and raised the need to plan multiplexing logic so multiple CAN expanders of the same type could coexist on a single bus.

## Details

### Implementation

- **16 additional ADC channels** exposed to rusEFI/epicEFI over CAN bus
- Expander hardware: **Arduino Mega2560** used as the CAN-connected ADC node
- The Mega2560 reads its analog inputs and transmits the values over CAN to the main ECU

### Planned Extension: Multiple Expanders of the Same Type

- @mitch0s raised the design consideration of multiplexing multiple CAN expanders of the same type
- Goal: allow users to add more than one Mega2560 ADC expander (or similar) on the same CAN bus
- Requires an addressing/multiplexing scheme to differentiate between expanders, since they would normally transmit on the same CAN IDs
- This was described as a planning/design-phase discussion — implementation was not yet complete at the time
