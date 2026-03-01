# Minimum Injector Pulse Width / Minimum Fuel Volume

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312*
*Contributors: @ggurov, @SanGawku, @kostasl*

## Summary

Fuel injectors have a physical minimum fuel volume below which they cannot deliver a consistent, reliable amount of fuel. rusEFI/epicEFI implements a minimum pulse width threshold (`minpw`) so the ECU never commands an injector to open for less than this duration when injection is active. This behavior is analogous to the minimum pulse width setting found on Honda ECUs and is also present in MaxxECU.

## Details

### Minimum Fuel Volume Concept

Injectors have a "minimal fuel volume" — a lower bound on how little fuel can be reliably delivered in a single injection event. Attempting to inject less than this quantity produces inconsistent results because the injector tip, needle dynamics, and spray physics are not linear at very short opening durations.

> "injectors have a 'minimal fuel volume'"
> — @ggurov

> "yeah thats like minimum pulsewidth on hondas"
> — @SanGawku

The setting is well-known across multiple ECU platforms:

> "Maxxecu has that too"
> — @kostasl

### Practical Rule

The intent is: if you are going to inject at all, don't attempt to inject less than the minimum. The threshold only applies when injection is commanded — it does not force injection when the calculated demand is zero (e.g., during decel fuel cut).

> "like, don't bother trying inject less than this if you inject any"
> — @ggurov

### Implementation Formula

The rusEFI/epicEFI firmware calculates the final pulse width using:

```
final_pw = max(calcpw, minpw)  if calcpw > 0
```

- `calcpw` — the pulse width calculated from VE table, fuel model, and corrections
- `minpw` — the configured minimum pulse width floor
- If `calcpw` is zero (injection disabled / DFCO), the minimum is not applied — the injector stays closed

This ensures the injector either stays fully closed or opens for at least the minimum effective duration, avoiding the non-linear and inconsistent region at the bottom of the injector's operating range.
