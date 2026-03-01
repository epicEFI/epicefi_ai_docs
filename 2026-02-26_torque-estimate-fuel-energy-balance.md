# Torque Estimation via Fuel Energy Balance

*Source: rusEFI Discord, 2026-02-26 | Channel: 1401895238481744052 (general-tech)*
*Contributors: @ggurov*

## Summary

A torque estimation method based on fuel energy balance is implemented in rusEFI/epicEFI. It converts the chemical energy of injected fuel into an estimated shaft torque using brake thermal efficiency, and requires a one-time calibration against a known dyno measurement.

## Details

### Formula

```
Torque [Nm] = eta * m_fuel_total [g] * LHV [J/g] / (4π [rad/cycle])
```

Where:
- **eta** (`torqueEstimateEfficiency`) — brake thermal efficiency (dimensionless, typically 0.25–0.38 for gasoline engines at wide-open throttle)
- **m_fuel_total** — total fuel mass injected per cycle (grams), derived from injector pulse width and injector flow rate
- **LHV** — lower heating value of the fuel (J/g); for gasoline: ~43,400 J/g; for E85: ~27,900 J/g
- **4π** — conversion factor: 2 revolutions per 4-stroke cycle × 2π rad/revolution

### Calibration

The efficiency coefficient `torqueEstimateEfficiency` must be calibrated against a known dyno run:

1. Perform a dyno pull to obtain measured torque at one or more operating points
2. Log `m_fuel_total` and the calculated raw torque estimate at the same operating points
3. Adjust `torqueEstimateEfficiency` until the estimate matches the dyno measurement
4. The calibrated value can then be used across the RPM/load range, with the estimate tracking changes in fueling

### Practical Notes

- This is a **virtual torque channel** — it does not require a torque sensor or dynamometer for real-time use once calibrated
- Accuracy depends on: injector calibration accuracy, fuel LHV consistency, and how well brake thermal efficiency tracks across the operating range
- The estimate is most accurate near the calibration point (typically peak torque or WOT at a specific RPM); extrapolation to light load / part throttle introduces error as thermal efficiency varies
- Useful for: torque-based launch control thresholds, traction control scaling, gear shift strategies, and data logging

## Notes

- The code comment in the firmware reads:
  ```
  // --- Torque estimate via fuel energy balance ---
  // Uses brake thermal efficiency to convert fuel chemical energy to shaft torque.
  // Torque [Nm] = eta * m_fuel_total [g] * LHV [J/g] / (4π [rad/cycle])
  // Calibrate torqueEstimateEfficiency against a known dyno run.
  ```
- LHV values: gasoline ~43,400 J/g; E10 ~42,900 J/g; E85 ~27,900 J/g; ethanol ~26,800 J/g
