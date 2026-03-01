# FuelFlowRateSmoothed Channel and Injector Smoothing Alpha Setting

*Source: rusEFI Discord, 2025-11-24 | Channel: tuning-and-software (1356732771325968630)*
*Contributors: Robb235 (@robb235_00916), ggurov (@ggurov)*

## Summary

A new `FuelFlowRateSmoothed` output channel was added to epicEFI firmware (released 2025-11-24) to provide a low-pass-filtered fuel flow rate for use in real-time gauges and custom expressions. The smoothing alpha follows the same convention as the lambda smoothing filter: a value of 1.0 means no smoothing; lower values apply progressively heavier filtering. Users with custom instant-MPG or fuel economy expressions need to update their expressions to reference `FuelFlowRateSmoothed` instead of `FuelFlowRate`.

## Details

### Background

Robb235 was trying to get a smooth instant-MPG reading on TunerStudio. The raw `FuelFlowRate` channel is noisy enough to make a real-time MPG gauge hard to read. ggurov added a smoothed variant to the firmware output channels.

### The Smoothing Alpha Setting

The smoothing amount is configured in **Injector Preferences** via a dedicated smoothing field. The alpha value follows exponential moving average (EMA) semantics, identical to how lambda smoothing works in rusEFI/epicEFI:

| Value | Effect |
|-------|--------|
| `1` (or `1.0`) | No smoothing — raw value passes through unchanged |
| `0.03` | Light smoothing — recommended starting point |
| `0.01` | Moderate smoothing |
| `0.001` | Heavy smoothing — may be too aggressive for fuel flow display |

- A lower alpha retains more of the previous value, producing a smoother but lagged output.
- Robb235 tested `0.01` and `0.001`; found `0.001` was too much for his use case and settled on `0.03`.

### Locating the Setting in TunerStudio

1. Go to **Injector Preferences**.
2. Find the smoothing field in the injector section.
3. If the field does not appear after a firmware update, close and re-open TunerStudio (TS can fail to pick up INI changes without a restart). If it still does not appear, manually reload the `.ini` file.

### Updating Custom Expressions

If you have a custom gauge or expression that references `FuelFlowRate` directly (e.g., a custom instant-MPG channel), update the variable name:

- Before: `FuelFlowRate`
- After: `FuelFlowRateSmoothed`

The unsmoothed `FuelFlowRate` channel remains available for logging purposes.

### Firmware Availability

The `FuelFlowRateSmoothed` channel and the injector smoothing setting were included in the epicEFI firmware release available on 2025-11-24. ggurov confirmed the build was available on the content server for all ECU variants on that date.
