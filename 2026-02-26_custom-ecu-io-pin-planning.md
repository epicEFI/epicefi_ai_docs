# Custom ECU I/O and Pin Planning for rusEFI/epicEFI Hardware

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @offtheband, @ZeeKay, @GGGI, @SanGawku*

## Summary

Community members discussed pin allocation and I/O planning for a custom rusEFI/epicEFI ECU design. Topics covered sensor ground pin counts, adding I/O to STM32 hardware, a concrete I/O channel list for a full-featured build, and the bare minimum pin requirements for a basic setup.

## Details

### Sensor Ground Pin Allocation

A connector was observed where pins 23 through 34 (12 pins total) are all designated as sensor ground. Community input noted that as few as 5 sensor ground pins could be sufficient for most setups, freeing the remaining pins for general-purpose I/O use.

### Adding I/O to STM32

If the STM32 has unused I/O pins on-chip, those can be repurposed directly. Adding entirely new I/O (beyond what the MCU natively exposes) requires additional circuitry:
- Voltage dividers for level shifting
- Protection circuits (ESD, overcurrent)

### Full-Featured I/O Channel List (GGGI proposal)

A comprehensive I/O layout for a full-featured engine management build:

**Primary channels:**
- 6x ignition outputs
- 6x injector outputs
- 3x Hall sensor inputs (crank, cam, VSS)
- 1x VR crank sensor input
- 6x low-side outputs
- 2x high-side outputs
- 1x Wideband O2 (WBO) channel
- 2x Electronic Throttle Body (ETB) channels
- 1x CAN bus channel
- 2x spare analog inputs
- 2x spare temperature analog inputs
- IAT, CLT, PPS1, PPS2, TPS1, TPS2 analog inputs

**Expandable (secondary board / IO expander):**
- 6x additional ignition outputs
- 6x additional injector outputs
- 3x additional Hall sensor inputs
- 4x additional low-side outputs
- 2x additional high-side outputs
- 1x additional CAN bus channel
- 1x additional VR input

### Minimum Pin Requirements

For a bare-minimum engine management setup, at least 5 high-side and 5 low-side output pins are required.

### I/O Expansion via Separate Module

The concept of using a dedicated secondary processor board purely for I/O expansion was discussed. This approach gives the main ECU more headroom by offloading digital and analog I/O management to a co-processor — effectively "an IO box with enough brains to run an engine."
