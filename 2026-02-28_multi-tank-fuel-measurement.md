# Multi-Tank Fuel Measurement and Consumption Monitoring

*Source: epicEFI Discord, 2026-02-28 | Channel: 1401895238481744052*
*Contributors: @ggurov*

## Summary

rusEFI/epicEFI supports a multi-tank fuel measurement system that tracks consumption across up to 3 independent fuel tanks, provides 9 derived readings, measures fuel by mass (not volume), and includes a calibration fudge multiplier for real-world accuracy tuning.

## Details

### Tank and Measurement Structure

- The system is configured with **3 fuel tanks** and **3 corresponding trip fuel measurements** (one per tank).
- From these inputs, the system produces **9 readings total**, covering the combination of per-tank trip fuel data with derived metrics.
- For each tank, the system reports:
  - Trip fuel consumed from that tank
  - Percentage of tank capacity used

### Fuel Mass Accounting

Fuel consumption is tracked **by mass (grams), not by volume (liters)**. This matters because:

- Ethanol and gasoline have different energy densities and densities by volume.
- The system performs a **liters-to-grams conversion using the configured ethanol percentage**.
- A flex-fuel or ethanol-blended setup will produce different gram-per-liter figures than a pure gasoline setup, and the system accounts for this automatically once ethanol % is configured.

### Calibration

- The system includes a **fudge multiplier** in the configuration to correct for real-world discrepancies between calculated and actual fuel consumption.
- This multiplier requires tuning — it should be adjusted by comparing logged fuel consumption totals against known fill-up volumes over time.

## Notes

- Accurate ethanol percentage input (from a flex-fuel sensor or manual configuration) is required for the L-to-g conversion to be meaningful.
- The fudge multiplier is a coarse correction tool; systematic error (e.g., injector characterization, dead-time tables) should be addressed at the source rather than relying solely on this multiplier.
