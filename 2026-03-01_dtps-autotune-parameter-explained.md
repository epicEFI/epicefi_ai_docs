# dTPS in Auto Tune — What It Means and Why It Matters

*Source: epicEFI Discord, 2026-03-01 | Channel: 1356732771325968630*
*Contributors: @Robb235, @Joesphan*

## Summary

dTPS stands for "delta throttle position sensor" — the rate of change (derivative) of the TPS signal. In TunerStudio's VEAL auto-tune, the dTPS threshold prevents the autotune algorithm from making VE corrections during transient throttle movements, where AFR data is unreliable.

## Details

The "d" prefix follows standard notation: d = delta (change) or derivative. dTPS therefore represents how quickly the throttle position is changing per unit time, not the absolute TPS value.

VEAL uses the dTPS threshold to distinguish steady-state fueling conditions from transient events (tip-in, lift-off). During a transient, the AFR reading at the wideband lags behind the actual in-cylinder mixture because of:

- The physical delay of exhaust gases reaching the sensor
- Acceleration enrichment fuel that does not represent steady-state VE

If the dTPS value exceeds the configured threshold, VEAL suspends corrections for that sample period. This prevents autotune from "chasing" the transient enrichment/enleanment signal and writing incorrect VE values into cells that are only briefly visited.

In practice, setting the dTPS threshold correctly means autotune will only update VE table cells when the throttle has been stable long enough for the wideband reading to reflect the true steady-state mixture.

## Notes

- The dTPS threshold interacts with the lambda delay table (stored in the TunerStudio project folder, not on the ECU) — that table tells VEAL how many milliseconds to wait after a throttle change before trusting the AFR reading.
- For best autotune results, do steady throttle cruise runs rather than frequent tip-ins; this keeps dTPS low and maximises the number of valid correction samples.
