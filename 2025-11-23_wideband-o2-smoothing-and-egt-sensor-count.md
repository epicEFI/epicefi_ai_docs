# epicEFI: Wideband O2 Signal Smoothing and EGT Sensor Support

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping), Joesphan (@joesphan)*

## Summary

Two brief feature confirmations for epicEFI: built-in wideband O2 signal smoothing (usable with any external wideband controller), and support for up to 8 EGT (exhaust gas temperature) sensor inputs.

## Details

**Wideband O2 signal smoothing:**

epicEFI includes a wideband O2 smoothing feature that applies filtering to the raw analog wideband signal from any external wideband controller. This allows noisy or electrically sensitive wideband sensors — including clone LSU 4.9 sensors and cheaper external controllers — to produce a stable, usable AFR signal for closed-loop fueling.

ggurov: "we have wideband o2 smoothing — so you can make any wideband behave, and get rid of the noise, to get proper signal that's not insane."

This is relevant for users who experience noisy AFR readings from budget wideband controllers (Tera noted issues with a clone LSU 4.9 on bank 2). The smoothing is a software filter applied in the ECU firmware, not a hardware solution.

**Context on the rusEFI-project wideband controller:**

A wideband controller exists within the rusEFI ecosystem: the hardware was designed by mck and the firmware written by dr0ngus. This is separate from Andrey Belomutskiy's single-channel wideband solution. Users can also use any third-party wideband (14point7 Spartan, AEM, Innovate, etc.) with the analog input smoothed by epicEFI.

**EGT sensor inputs:**

epicEFI supports up to **8 EGT (exhaust gas temperature) sensor channels**. This came up in context of a discussion about water/methanol injection for EGT cooling on a twin-turbo V8, where Tera noted needing temperature probes. ggurov confirmed the eight-channel EGT capability is available.

Eight individual per-cylinder EGT sensors is feasible on epicEFI — useful for monitoring exhaust temperature distribution on turbocharged engines or diagnosing misfire/lean cylinders by temperature delta.
