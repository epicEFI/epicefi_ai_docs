# Harness Wire Gauge Selection for Engine Management Systems

*Source: rusEFI Discord, 2025-11-25 | Channel: 1356732771325968630*
*Contributors: DCwerx (@dcwerx), MykkH (@mykkh), Jmoneus (@jmoneus)*

## Summary

The community discussed appropriate wire gauge choices for building ECU harnesses. The consensus is: use 22 AWG for most ECU signal wiring, 18 AWG exclusively for grounds to the intake manifold, and heavier gauges (12–10 AWG) only for high-current loads like fuel pumps and cooling fans. 18 AWG throughout is not recommended — it is oversized for signal wiring.

## Details

**DCwerx's harness baseline:**
- 22 AWG: used for almost everything in a modern ECU harness (signal wires, sensor feeds, low-current outputs)
- 18 AWG: reserved for grounds only, specifically grounds running to the intake manifold (multiple wires recommended for a solid ground plane)
- 18 AWG is described as "too big for almost everything except grounds" — it adds unnecessary bulk and stiffness to signal runs

**MykkH's gauge breakdown by current:**
- 16 AWG: "secret sauce" for general use up to ~10A (injectors, coils, solenoids)
- 12 AWG or 10 AWG: for high-current circuits drawing 25–30A, specifically fuel pumps and cooling fans

**Jmoneus (Hyundai Accent budget build):**
- Asked about using 18 AWG throughout to simplify sourcing and reduce cost
- DCwerx and MykkH advised against it: 18 AWG is oversized for signal runs and stiff to route
- Jmoneus settled on 10 AWG for high-current circuits due to an additional turbo scavenge pump raising peak load

**Practical summary table:**

| Application | Recommended Gauge |
|---|---|
| ECU signal wires, sensors | 22 AWG |
| Injectors, coils, solenoids (≤10A) | 16 AWG |
| Sensor grounds to intake manifold | 18 AWG (multiple runs) |
| Fuel pump, cooling fan (25–30A) | 12 AWG or 10 AWG |
| High-current loads with extra pumps | 10 AWG |

**Harness quality note:** DCwerx builds all-soldered harnesses (referenced a BMW E-series link G4+ build where every joint was hand-soldered) for long-term reliability. Crimp connections are acceptable but solder is preferred by experienced builders for permanence.
