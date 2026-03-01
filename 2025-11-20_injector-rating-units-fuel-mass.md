# Injector Rating Units, Fuel Density, and ECU Fuel Mass Calculations

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: Mr. Zebra (@mr.zebraaa), Joesphan (@joesphan), ZeeKay (@its_zeekay)*

## Summary

Discussion clarifying why aftermarket injectors are rated in cubic centimeters (cc/min or cc) rather than grams per second (g/s), and how this interacts with ECU fuel mass calculations and air-fuel ratio (AFR) accuracy.

## Details

**Injector flow rating units — cc vs g/s:**
Most aftermarket injectors are rated in cc (volume flow), not g/s (mass flow). This is physically correct: at a given fuel pressure, the injector orifice delivers a consistent volume of fuel. Temperature and fuel type do not change the volume flow at fixed pressure — they only affect fuel density.

- Injector rating: volume-based (cc) is the industry norm for aftermarket parts.
- `pressure + orifice = volume flow` — temperature/fuel type affect density, not volume delivered per pulse.

**Why the ECU needs fuel mass:**
Despite injectors being rated by volume, the ECU must work in mass units internally because stoichiometric AFR calculations require mass (not volume) ratios. This means:

- The ECU converts cc flow to mass using fuel density.
- If fuel temperature changes, fuel density changes, and therefore the injector pulse width (PW) must be adjusted to deliver the correct fuel mass for the target AFR.
- If fuel pressure remains constant, volumetric flow rate is static — but the mass delivered per unit time changes with temperature.

**Practical implication:**
On a well-configured system with fuel temperature compensation, injector pulse width will vary slightly with fuel temperature even at the same load point, because the ECU is targeting a fuel mass, not a volume. Systems without fuel temperature sensing will use a fixed assumed density, which can introduce small AFR errors in extreme ambient conditions.
