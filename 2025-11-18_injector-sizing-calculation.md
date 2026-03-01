# Injector Sizing Calculations for High-Performance Engines

*Source: rusEFI Discord, 2025-11-18 | Channel: 1379478511667908618*
*Contributors: @ggurov, @Robb235*

## Summary

Discussion correcting a commonly cited injector sizing formula and working through realistic flow rate requirements for a 600 hp V8, taking into account duty cycle limits and target AFR. The corrected calculation shows that 275 cc/min (from the popular formula) significantly undersizes the injectors, and a realistic figure is closer to 380–460 cc/min depending on target AFR.

## Details

### The Commonly Cited Formula and Its Problem

A frequently quoted rule of thumb for injector sizing is:

> "For example: A 600 hp V8 will require each injector to flow at least 600 hp x 5 cc/min/hp / 8 injectors = 600 x 5 / 8 = 275 cc/min." — @ggurov (quoting the formula)

@Robb235 immediately flagged this:

> "That math ain't mathing for me"

The formula produces 375 cc/min total (600 × 5 / 8 = 375), not 275 cc/min — the arithmetic in the quoted example is wrong. More importantly, the formula does not account for duty cycle or AFR targets, which are critical for determining actual required flow.

### Corrected Calculation at Stoichiometric AFR

@Robb235 worked through the numbers with duty cycle factored in:

> "I'm coming up with more like ~380cc/min. 85% duty cycle and 14.7 AFR. Which really it needs to be richer."

At 85% duty cycle and a stoichiometric AFR of 14.7:1, the required injector flow is approximately **380 cc/min per injector** for a 600 hp V8. The note that "it needs to be richer" reflects that a high-performance engine making 600 hp at WOT should not be running at 14.7 AFR — a richer mixture is needed for power and engine protection.

### Corrected Calculation at Power AFR

For a realistic power AFR:

> "Really to run 85% duty cycle and 12.0 AFR you need like 460cc/min per injector" — @Robb235

At 85% duty cycle and a target AFR of 12.0:1 (a typical WOT power AFR for naturally aspirated or mildly boosted engines), the required flow is approximately **460 cc/min per injector**.

### Summary of Results

| Condition | Duty Cycle | AFR | Required Flow |
|-----------|-----------|-----|---------------|
| Stoichiometric (street) | 85% | 14.7:1 | ~380 cc/min |
| Power enrichment (WOT) | 85% | 12.0:1 | ~460 cc/min |
| Rule-of-thumb formula result | — | — | 275 cc/min (understated) |

### Key Takeaways

- The popular "hp × 5 / cylinders" formula significantly undersizes injectors — do not use it as the sole basis for selection.
- Always factor in maximum duty cycle (80–85% is a safe upper limit for most injectors) and the richest target AFR the engine will see.
- For a 600 hp V8 targeting 12.0 AFR at WOT with 85% duty cycle, injectors rated at least **460 cc/min** are required.
- Running at lower duty cycles or requiring headroom for additional power potential would push the required rating higher.
