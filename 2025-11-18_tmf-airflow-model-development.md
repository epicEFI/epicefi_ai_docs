# TMF (Total Mass Flow) Airflow Model Development in rusEFI/epicEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312 (theory/dev)*
*Contributors: @ggurov, @fast335xi, @joesphan*

## Summary

An extended technical discussion covering the design and early implementation of a Total Mass Flow (TMF) airflow model in rusEFI/epicEFI. The work involves adding dual MAP sensors, implementing the throttle body pressure ratio equation (including choked vs. subsonic flow regimes), defining the sensor signal fallback chain, modeling TPS-to-effective-area scaling, and planning a self-tuning Discharge Coefficient (Cd) feature. A key reference data point was validated: stock DME logs showing 550 g/s TMF producing 27 psi at 6500 RPM were accurately reproduced by the pressure ratio equation.

## Details

### Background and Motivation

The channel was described as "a place to dump knowledge and discuss actual theory, keeping it out of general." The starting point was @ggurov announcing work on adding a second MAP sensor to attempt the TMF airflow model. @fast335xi noted that few ECUs implement TMF, and that it had been partially started in rusEFI firmware roughly two years earlier by "mck" but with essentially no code behind it ("half-baked, but there"). The goal was to implement it properly as a separate, clean model.

### What TMF Is

TMF (Total Mass Flow) is distinct from traditional speed-density (MAP-based) or MAF-based load estimation. It models the throttle body as a compressible flow orifice using the pressure ratio equation. Key concepts:

- **TMF**: air mass entering the intake manifold (input boundary at the throttle body)
- **IMF**: air mass entering the cylinders (output boundary)
- For idle: TMF sets airflow targets while IMF fine-tunes ignition control
- For boost: TMF predicts pressure demand and IMF determines if the demand is met
- @joesphan noted that manifold pressure alone is an inaccurate way of metering air

### Dual MAP Sensor Configuration

@ggurov added support for two MAP sensors in rusEFI:

- Both sensors are averaged/window-sampled the same way
- They do not have to be the same scaling
- One sensor is the standard manifold MAP; the second is used as the pre-throttle body (pre-TB) pressure input for the TMF calculations
- Previously, there was a "hidden" pre-TB pressure sensor input in the firmware — it logged data but was unused in calculations

### Sensor Signal Fallback Chain

For the pre-throttle pressure input, rusEFI uses the following fallback priority:

```
actual sensor -> preThrottle pressure (hidden sensor input) -> baro -> initial map reading -> hardcoded standard 101 kPa
```

The system defaults to 101 kPa standard atmospheric pressure if no sensor is present.

### Pressure Ratio Equation and Choked Flow

The core TMF airflow calculation uses a compressible flow / pressure ratio equation. An excerpt of the implementation shared by @ggurov:

```cpp
float TMFAirmass::getAirFlow(float manifold_pressure, float pre_tb_pressure, float tps, float tk) {
    float area = effectiveArea(tps); // this is in m^2
    float deltaP = std::max(pre_tb_pressure - manifold_pressure, 0.0f) * 1000.0f; // Pa - pascals
    float pratio = manifold_pressure / pre_tb_pressure;
    // ...
}
```

Two distinct flow regimes are handled:
- **Subsonic (non-choked) flow**: when pressure ratio >= ~0.528
- **Choked (sonic/critical) flow**: when pressure ratio < ~0.528 (when flow breaks Mach 1)

The choked flow threshold of 0.528 is the textbook value and was left as the default. The practical implication noted by @ggurov: below approximately 52 kPa manifold pressure (at standard atmospheric pre-TB pressure), airflow becomes the same regardless of how much lower it gets — which means VE should be modeled as flat in that regime rather than continuing to scale with MAP.

@fast335xi confirmed the pressure ratio equation matched stock DME logs: **550 g/s TMF at 27 psi boost at 6500 RPM**. This data point can be used to rearrange the equation and find mass flow values for building a base map.

### BMW Choked Flow Handling (for reference)

@fast335xi described how the BMW DME handles the transition from subsonic to choked flow:
1. Torque reduction controls the throttle area first
2. Then ignition retard is applied
3. In extreme conditions, fuel cut with individual cylinder cuts in 0.5-degree increments
4. A limit map ranges from 7–15 degrees
5. Fuel film compensation is also applied

### TPS to Effective Throttle Area Scaling

The throttle body flow area is not linearly related to TPS percentage. Key points from the discussion:

- A "throttle effective area" setting was already present in rusEFI (noted by @joesphan)
- The relationship is an S-curve (butterfly valve geometry)
- @ggurov added TB diameter as a configuration input and models TPS scaling with an offset for universality
- The formula for a butterfly valve: at 90 degrees = 100% open, at 0 degrees = fully closed
- Rather than hardcoding a formula, a tunable curve is used: "drive around gathering data from other models and make this one fit"
- @fast335xi found a BMW map for TPS-to-effective-area (S-curve) that was used as the starting reference

### TMFLoadIsTMF Configuration Variable

@ggurov added a boolean configuration variable:

- **Variable**: `TMFLoadIsTMF`
- **When `true`**: use 100% VE-based "cylinder fill" calculations for engine load
- This allows the load axis for ignition and other tables to be based on cylinder fill percentage (pure VE model) rather than raw manifold pressure

### Discharge Coefficient (Cd) Auto-Calibration Plan

After basic operation is established, @ggurov planned to add a "suggested Cd" feature:

- Will reference airflow numbers from the speed-density or MAF model
- Combined with EGO (wideband O2) sensor feedback
- Goal: automatically generate a tuned Cd table from logged data rather than requiring manual calibration

### Speed Density as Interim Strategy

While the TMF model is being developed and validated, the recommended approach is:

1. Get the vehicle running on speed density mode first
2. Run the TMF model in parallel (logging calculations without using them for fueling)
3. Compare logged TMF calculations to speed density values
4. The TMF calculations will eventually go through a similar processing structure as MAF/MAP data (blend mechanism: "MAP/TMF blend")

### Required Hardware

To fully utilize the TMF model:
- Pre-throttle body pressure sensor (upstream of the TB)
- Standard MAP sensor (downstream, in the manifold)
- MAF sensor (helpful but not strictly required initially)

### Torque Request Concept

@fast335xi raised the idea of implementing a torque request input for traction control and transmission integration:
- A purely torque-based strategy with CAN input/output for DSC (Dynamic Stability Control) or TCU torque requests
- @ggurov's initial response: "that seems like a number to tb% override" — i.e., torque request would be implemented as a throttle percentage modifier
- The existing traction control implementation uses TB modifier, ignition skip, and ignition retard based on wheel slip

### VE Lookup Tables Referenced from BMW OEM Data

@fast335xi shared several BMW OEM lookup tables that were discussed as references for the TMF model implementation:

- **VE lookup based on intake cam angle**: A table correlating camshaft angle to VE values for VANOS compensation
- **Pressure-to-load conversion table**: Converting pressure readings to engine load values
- **Boost pressure correction factor**: A correction map for VE under boost conditions
- **Throttle control transient stabilization with high pressure ratios**: Maps handling transient behavior under boost

These were shared as image and TSV file attachments to inform the rusEFI TMF model calibration approach.

### OEM VE Maps Discussion

@fast335xi shared BMW OEM VE maps showing:
- Two VE maps: one for normal operation, one for emergency/limp/cold operation
- Values can exceed 100% around peak torque rpm (expected for a highly optimized engine)
- @ggurov asked about the blend logic between the two maps; @fast335xi noted the lower one is likely for cold/emergency/limp operation

### References

- Emtron TMF documentation: https://help.emtronaustralia.com.au/emtune/Newtopic33.html
- BMW XDF files repository (S55, B58 maps): https://github.com/dmacpro91/BMW-XDFs
