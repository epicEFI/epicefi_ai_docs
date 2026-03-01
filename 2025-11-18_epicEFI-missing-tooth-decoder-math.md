# epicEFI Missing Tooth (60-2) Decoder Math and AEM Virtual Tooth Wheel

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @ggurov, @Mitch (mitch0s)*

## Summary

ggurov walks through the fundamental math for a 60-2 missing tooth decoder, explains how TDC is located using the missing teeth as a reference plus a configurable offset, and describes how AEM implements a "virtual 24-tooth wheel" as a universal event mapping mechanism for trigger wheel abstraction. These concepts form the basis for epicEFI's planned missing tooth decoder implementation.

## Details

### 60-2 Missing Tooth Decoder Basics

A 60-2 wheel has 60 base tooth positions with 2 teeth removed to create a distinctive gap.

**Degrees per tooth calculation:**
```
360 degrees / 60 teeth = 6 degrees per tooth
```

**TDC location:**
The gap created by the 2 missing teeth serves as the synchronization reference. TDC is located by:
```
TDC position = (position of missing teeth gap) + offset
```
The offset is a configurable value that accounts for the physical mounting angle of the trigger wheel relative to the crankshaft's actual TDC.

**Why teeth counting matters at cranking:**
During initial cranking, tooth-to-tooth durations start near zero and increase as the engine accelerates. Teeth counting (rather than pure timing) avoids false TDC detections during this phase. Fine-tuning the counting thresholds for a specific engine is required.

### AEM Virtual 24-Tooth Wheel

ggurov described a technique used by AEM ECUs to create a generic event-mapping layer for any trigger wheel:

1. Internally, the ECU "spins" a virtual 24-tooth wheel
2. A lookup table maps each **physical tooth** from the actual trigger wheel to a position on the virtual 24-tooth wheel
3. Injector and coil phasing for any firing order is then configured against the virtual wheel positions

This allows the ECU to support arbitrary firing orders and trigger wheel configurations by changing the mapping table rather than the core event scheduling code.

```
Physical trigger wheel teeth  →  Event table  →  Virtual 24-tooth positions  →  Output scheduling
```

ggurov notes this took significant reverse-engineering effort to understand: "it took a while to figure that shit out."

### Multi-Cylinder Offset Model

For multi-cylinder engines, each cylinder is treated as an independent single-cylinder engine with its own angular offset from the shared TDC reference. No special multi-cylinder logic is needed at the decoder level — the offset values in the cylinder configuration account for firing order and cam phasing.

### Implications for epicEFI

The 60-2 decoder is identified as the best starting point for implementing the generic inter-tooth update approach (`updateTiming()`):
- Tooth positions are uniform (6 degrees each), making angle calculation straightforward
- The missing gap is easy to detect via tooth-time ratio comparison
- It is used on a large number of European engines

The discussion explicitly prioritized 60-2 as "the most generic" decoder to tackle first, given its widespread use and well-understood structure.
