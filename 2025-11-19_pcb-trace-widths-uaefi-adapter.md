# PCB Trace Width Guidelines for uaefi Adapter Board
*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @kostasl (kostasl0737), @MarkCraftPerformance (carolinacumberland)*

## Summary

Discussion on appropriate PCB trace widths when designing an adapter board for the uaefi platform. General guidance: main power traces (~5 mm), with heavier traces for 12V, GND, and half-bridge outputs. Exact sizing depends on the MCU and intended application.

## Details

### Context

@kostasl was designing a custom adapter board for uaefi and needed guidance on trace widths before routing, specifically asking about:
- Power (12V and GND) trace sizing
- Half-bridge trace sizing

### Guidance

- **General rule**: Main traces approximately **~5 mm wide** as a starting point.
- **Power traces (12V, GND)**: Should be **thicker/beefier** than signal traces to handle current capacity.
- **Half-bridge traces**: Should also be sized heavier given the current they carry.
- **Actual sizing**: Depends on the specific MCU and the application's current requirements. Higher current applications need wider copper.

### Recommended Approach

Use the existing **uaefi reference design** trace widths and copper thickness as a baseline. Match or exceed those values for any adapter board that connects to uaefi hardware. Standard PCB trace width calculators can help verify that a given width handles the expected current without excessive temperature rise.

### Also Noted

- When the PCB corner flat sections are too wide (e.g., on connector footprints or mechanical keepout areas), they can physically interfere with nearby components such as capacitors and resistors near H-bridge sections. Trimming these corner areas in the PCB layout resolves the interference.
