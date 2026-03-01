# MCP2515 CAN Controller Integrated Board Design for epicEFI

*Source: rusEFI Discord, 2025-11-27 | Channel: 1434034231713075282*
*Contributors: Walter R. (@walterronny), GGGI (@.gunadi)*

## Summary

Community members are developing a custom PCB integrating an Arduino Mega 2560 with an MCP2515 SPI-to-CAN controller into a single board for use with epicEFI. The design approach favors a shield/carrier form factor (Mega sitting on top) over a fully integrated SMD board, to allow easier rework and modular replacement. A reference design exists at the CANBoard_HW GitHub repository.

## Details

### Design Goals

- Single board combining Arduino Mega 2560 and MCP2515 CAN controller
- Intended to add CAN bus capability to epicEFI setups requiring additional digital inputs
- Board to be shared with the epicEFI community once complete

### Form Factor Decision

Two approaches were considered:

| Approach | Notes |
|----------|-------|
| Fully integrated (Mega + MCP SMD on same PCB) | Compact but harder to rework |
| Shield/carrier (Mega plugs into carrier board, UA4C style) | More space for additional components, easier to replace Mega if damaged |

GGGI favored the shield/carrier approach modeled on the UA4C board layout, citing more room for peripherals and easier repair. Walter R. had prior experience building a custom Speeduino carrier board with the Mega on a separate plug-in module.

### MCP2515 Schematic

GGGI had a partial KiCad schematic (approximately 50% complete at the time of discussion) based on reference images found online. The schematic was noted as unverified but cross-checked against multiple reference images. Walter R. asked for the schematic file; GGGI shared it directly in Discord.

A note from the schematic: the dual-diode protection circuit (referenced as "dual diode thingy from UA4C") had not yet been added to the MCP2515 section at the time of sharing.

### Reference Design

An existing small-form-factor CAN board was identified as a useful reference:

- **CANBoard_HW** by corygrant: https://github.com/corygrant/CANBoard_HW
- Noted as compact and well-suited as a starting point

### KiCad Workflow Notes

- Net labels are sufficient for connecting nets within a single-sheet schematic; global labels are only needed for hierarchical multi-sheet designs
- Component footprints for less common parts: LCSC (lcsc.com) is a recommended source for pulling footprints into KiCad rather than relying on user-contributed library entries
- Walter R. noted prior Speeduino custom board experience as background for the Mega integration design
