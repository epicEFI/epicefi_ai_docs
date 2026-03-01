# tCharge, IAT, and the Speed Density Air Mass Calculation

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111 (advanced-tuning)*
*Contributors: @ggurov, @Joesphan, @Jordan, @Robb235*

## Summary

A detailed discussion clarifying how intake air temperature (IAT) interacts with the Speed Density (SD) VE equation in rusEFI/epicEFI. The key insight is that IAT is not passed directly into the SD calculation — instead, a derived value called `tCharge` (charge air temperature) is used. tCharge is computed via a moving average model that estimates air temperature at the intake ports, accounting for heat soak from coolant temperature. The UI displays this as "ideal gas law temp" in the bottom right of the tuning interface. A separate IAT multiplier table exists as a post-calculation correction to fuel mass.

## Details

### IAT is Not a Direct Input to the SD VE Equation

IAT is not passed directly to the Speed Density VE equation. Instead, a computed value `tChargeK` is used:

```cpp
float airMass = getAirmassImpl(ve, map, tChargeK);
// from: AirmassResult SpeedDensityAirmass::getAirmass(float rpm, float map, bool postState)
//   float tChargeK = engine->engineState.sd.tChargeK;
```

The `tChargeK` value is computed before it reaches the VE equation, meaning IAT alone does not drive the calculation. This is why rusEFI's air mass model is described as more advanced than simpler systems like Speeduino or MS3.

### What is tCharge?

tCharge (charge air temperature) is a moving average model that attempts to predict the actual temperature of air at the intake ports — after air has traveled through the intake runners and been heated by the engine. It is a function of both IAT and coolant temperature (CLT):

- At idle, air slows down and heats up more on the way to the cylinders
- tCharge increases as CLT increases
- IAT is treated as a static/baseline reference; the model blends it with CLT effects

Example from a simulator run: at 3500 RPM with CLT at -24°C and IAT at 28°C, tCharge will differ from raw IAT.

### Monitoring tCharge in the UI

The temperature value fed into the SD equation can be monitored directly in the tuning software. It appears in the bottom right of the interface labeled **"ideal gas law temp"**, which corresponds to `tCharge`. This is what is actually used for air mass calculations, not the raw IAT reading.

### IAT Multiplier Table — Post-Calculation Correction

There is a separate IAT multiplier table that acts as a correction to fuel mass after the core SD calculations are complete. This table:

- Is **disabled by default**
- Applies a fuel mass correction based on IAT after the main air mass calculation
- Is separate from the tCharge modeling
- Is not the same as the tCharge blending table

The tCharge blending table (visible in the interface) allows monitoring of the temperature sent into the SD equation. It models how much air heats up traveling from the IAT sensor to the intake ports, influenced by CLT.

@ggurov described the model purpose:
> "it's attempting to model how much air heats up from iat to ports via clt"

### The IAT Fuel Adder Debate

A sub-discussion arose about whether the IAT multiplier table (fuel adder) was necessary:

- **@Jordan**: questioned the use case of a separate IAT fuel adder outside the air mass calculation. Noted that if you want richer mixture at high IAT (for knock protection/cooling), you should change the AFR/lambda target instead of adding raw fuel, otherwise the measured AFR won't match target.
- **@Robb235**: pointed out that STFT would remove any extra fuel added if the AFR target is already being met.
- **@Joesphan**: clarified that STFT will indeed remove the added fuel if it doesn't help hit the AFR target — "Stft will take the fuel out if it doesn't hit afr target, which in theory it should because more oxygen."

The correct approach for protecting the engine from hot inlet air is to adjust the lambda/AFR target based on IAT, not to add fuel via the multiplier table.

### Tuning the IAT Correction Table

To tune the IAT-based correction table:
1. Take a log with varying IAT values (e.g., using a hair dryer on the intake to artificially raise IAT)
2. Observe the "ideal gas law temp" (tCharge) in the UI vs. IAT
3. Tune the correction accordingly

The effect is most pronounced at idle where airflow is slow and heat soak from the engine is greatest.

### Manifold Temperature Measurement

@Joesphan raised the question of whether the IAT correction approach is made redundant if you simply measure manifold temperature directly. The suggestion was to install a thermistor or NTC sensor directly in the intake manifold to measure actual charge temperature rather than relying on a remote IAT sensor and a model.
