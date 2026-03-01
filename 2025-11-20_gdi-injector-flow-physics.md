# GDI Injector Flow Physics: Fuel Density, Units, and Square-Law Pressure Relationship

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, Joesphan, ZeeKay, Mr. Zebra*

## Summary

GDI injector flow rate is often rated in volumetric units (cc/min or cc/pulse) but mass flow is what the ECU actually uses for fuel calculations. Flow through a fuel injector (an orifice) follows a square-law relationship with pressure: doubling the pressure increases flow by only ~41% (sqrt(2) ≈ 1.41), not 2x. This has significant implications for low-pressure GDI operation and injector sizing.

## Details

### Fuel Density

- Gasoline density: **0.762 g/cc** (grams per cubic centimeter).
- This value is used to convert between volumetric flow (cc/min, cc/pulse) and mass flow (g/s, g/pulse) for fuel calculations.
- Example: Bosch HDEV 5.1 injectors (used in BMW N63TU) are rated at **15.9 g/s at 100 bar**.

### Square-Law (Orifice) Flow Relationship

- Flow through an injector nozzle (orifice) is proportional to the **square root of the pressure differential**, not pressure directly:

  ```
  Q ∝ sqrt(ΔP)
  ```

- Practical implication: going from 50 bar to 200 bar (4x pressure increase) only gives **2x** flow (sqrt(4) = 2).
- At reduced pressure (e.g., using only the intank low-pressure pump at ~3–5 bar instead of the HPFP at 50–200 bar), flow is dramatically reduced — not just proportionally. You cannot simply multiply pulsewidth by the pressure ratio to compensate.
- ggurov: "flow through an orifice is square law — so not getting a whole lot more fuel from that pressure [at 36 bar spike]. Just ability to overcome compression."

### Volumetric vs. Mass Flow — Why It Matters

- Injectors are commonly rated in **cc/min** (volumetric) by aftermarket suppliers, but the ECU uses **mass** (grams) for stoichiometric calculations.
- Converting: `mass_flow (g/min) = volumetric_flow (cc/min) × density (g/cc)`.
- Temperature affects density (and therefore mass flow) but does not change volumetric flow for a fixed pressure and orifice. ECU fuel models should account for fuel temperature effects on density if precision is needed.
- ZeeKay: "pressure + orifice = volume, not mass — temps/fuel types affect density but the injector flows the same volume."
- Mr. Zebra: "fuel mass is actually used in the fuel calculations in the ECUs."

### Injector Sizing in epicEFI

- For the N63TU on epicEFI: injector size in the tune should be set in a way that, at normal operating rail pressure and deadtime, the calculated pulsewidth lands in a workable range (ideally 1–10 ms).
- Deadtime (injector opening latency) must be configured; the default table is 1 ms across the board as a starting point.
- Cranking fuel enrichment in epicEFI uses a `req_fuel`-based multiplier. The cranking fuel table entries should be set to approximately 70 (representing 70% of req_fuel) as a baseline; adjust based on logged injection pulsewidth during cranking (target ~1.4–2 ms for GDI).
- Fuel cut code 12 = no sync; code 10 = max duty cycle exceeded. Both will prevent fueling.
