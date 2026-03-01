# GDI8 MOSFET Measurement Procedure: Vds, Vgs, and Scope Isolation

*Source: rusEFI Discord, 2025-11-24 | Channel: 1356732771325968630*
*Contributors: Joesphan (@joesphan), Tera (@teraflopping)*

## Summary

Detailed procedure for using an oscilloscope to measure MOSFET drain-source voltage (Vds) and gate-source voltage (Vgs) on the GDI8 board, including scope isolation requirements, expected voltage values, and how to correctly place probe grounds when measuring injector output signals. Includes the GDI8 boost rail voltage spec and MOSFET on-resistance spec.

## Details

### GDI8 Circuit Architecture

The GDI8 outputs a high-voltage boost rail to drive GDI injectors. The circuit works as follows:

- The **low-side (LS) MOSFET** grounds one pin of the injector.
- The **high-side (HS)** drives the other injector pin to approximately **70V** from the boost rail.
- VBoost is rated at a maximum of **72V**.
- The GDI8 runs on a **Blue Pill (STM32F103)** microcontroller.

### MOSFET Specifications

- On-resistance (Rds_on): **76 mΩ** (0.076 Ω)
- Expected Vds when the MOSFET is fully on: **near 0V**
- Example voltage drop calculation: 10 A × 0.076 Ω = **0.76 V** drop at 10 A conduction

If Vds measures significantly above this value when the device is conducting, the MOSFET is not being driven fully into saturation, which may indicate insufficient gate drive voltage.

### Scope Isolation Requirement

**The oscilloscope must be isolated from the GDI8 ground when making measurements on the board.**

The GDI8 boost rail operates at up to 72V, and the injector output floats relative to chassis ground during switching. If the oscilloscope's probe ground strap is connected to chassis or mains earth while measuring GDI8 signals, it will create a short through the scope's ground path. Use an isolation transformer or a battery-powered/isolated scope, or remove the scope's earth ground via a modified extension cord (without the earth pin connected).

### Measuring Vds (Drain to Source)

1. Connect the oscilloscope probe **positive tip to the MOSFET drain**.
2. Connect the **probe ground strap to the MOSFET source** (not to chassis ground or GDI8 power ground).
3. When measuring the LS MOSFET, the source is the drain-side injector pin (the low-voltage side).
4. The Vds should read **near 0V** when the MOSFET is fully on (conducting).

### Measuring Vgs (Gate to Source)

- Gate drive voltage must be sufficient to fully enhance the MOSFET into saturation.
- If Vds is higher than expected (e.g., several volts instead of <1 V), measure Vgs to confirm the gate is receiving adequate drive.
- Both Vds and Vgs can be measured simultaneously with a dual-channel scope. Keep both probe grounds at the same node (MOSFET source) to avoid ground loop issues.
- Only one probe ground strap is needed; remove the second if it is not in use, to prevent it from dangling and accidentally contacting a live node.

### Measuring Injector Output Waveform

To view the injector drive waveform with correct amplitude:

1. Place the **probe ground strap on one injector pin** (the LS MOSFET source / ground-side pin).
2. Place the **probe tip on the other injector pin** (the HS pin, which swings to ~70V).
3. Do not connect the ground strap to chassis ground — let the measurement float with respect to the injector.
4. The waveform should show the HS boost pulse rising to approximately **70V**, with small flyback spikes visible at switching edges. The flyback spikes are normal and expected; focus on the main pulse shape and duration.

### Diagnosing Half-Voltage Issue

If the measured injector output shows only half the expected voltage:

- First confirm the probe ground is placed correctly on an injector pin, not on chassis or GDI8 power ground.
- Measure Vds across the LS MOSFET while firing.
- Measure Vgs to verify sufficient gate drive.
- The most common cause of this reading was **incorrect probe ground placement** — the scope was not referenced to the injector node.
