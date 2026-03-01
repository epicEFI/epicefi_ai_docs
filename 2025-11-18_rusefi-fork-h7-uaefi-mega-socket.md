# rusEFI Fork — H7 Processor and UAEFI Mega Socket Configuration

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov*

## Summary

Brief notes from @ggurov on their fork of rusEFI firmware. All ECUs in this setup that use the Mega socket form factor use an STM32H7 processor. The fork maintains a common firmware image shared across both H7 and UAEFI variants.

## Details

### Hardware Configuration

- All ECUs fitted with the **Mega socket** form factor in this deployment use the **STM32H7** processor.
- The fork runs a **shared firmware** that covers both H7-based and UAEFI (Ukrainian EFI) boards.

### Firmware Compatibility Note

The shared firmware approach means the same build targets both H7 and UAEFI hardware variants. This simplifies maintenance but requires the firmware to handle differences between the two hardware targets within a single codebase.
