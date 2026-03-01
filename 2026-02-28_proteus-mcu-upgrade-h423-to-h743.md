# Proteus ECU Hardware Modification: Adding CAN Channel, Second WBO2, and MCU Upgrade

*Source: rusEFI Discord, 2026-02-28 | Channel: 1411925674498719775*
*Contributors: @SanGawku*

## Summary

@SanGawku was planning a hardware modification to a Proteus ECU board: adding a second CAN bus channel, a second wideband oxygen (WBO2) sensor circuit, and swapping the microcontroller from the STM32H423 to the STM32H743.

## Details

The Proteus ECU in its standard configuration ships with a single CAN channel and a single WBO2 controller. For advanced applications — particularly transmission control integration or dual-bank wideband monitoring — these can be insufficient. @SanGawku identified three changes needed:

1. **Second CAN channel:** Adding a second CAN transceiver and routing to an additional connector position allows a separate CAN bus for a TCU, ABS module, or data logger without sharing bus bandwidth with the primary engine management network.

2. **Second WBO2 circuit:** A second wideband oxygen sensor channel enables per-bank AFR measurement on engines with two exhaust banks (V-type engines, or separate manifolds). Each WBO2 controller operates independently.

3. **MCU swap: STM32H423 to STM32H743:** The H743 is a higher-capability variant in the STM32H7 family. Compared to the H423, the H743 offers more flash memory, more RAM, and additional peripheral instances (including additional SPI, UART, and timer channels), which can support the expanded I/O requirements of the modified board.

This modification is a hardware-level board rework, not a firmware-only change, and requires soldering and schematic knowledge.

## Notes

- This was a brief statement of planned work rather than a completed project report. No further details on the modification procedure were shared in this session.
- The STM32H743 is pin- and software-compatible with the H723 and H753 variants, but not necessarily drop-in compatible with the H423 without schematic review — pin mapping differences exist between the two package families.
