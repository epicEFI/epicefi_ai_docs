# GDI Injection Duration Error: PT2001 Limits and Diagnosis (Code 9013 / P06118)

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

On GDI (gasoline direct injection) engines using the PT2001 injector driver, excessively long injection pulse widths trigger an error condition (logged as "9013 too long fuel injection"). This is a hardware-level protection in the PT2001 driver IC — the ECU firmware detects when requested injection duration exceeds the driver's safe operating limits and cuts that injection event. A separate transient code, P06118 (crank position sensor), is normal on initial startup and can be ignored. The `injections1`–`injections8` monitor variables provide a per-injector fire counter useful for confirming which injectors are actually activating.

## Details

### Error Code 9013 — Injection Duration Excess

Code `9013` is reported by the firmware when the calculated injection pulse width exceeds the maximum permitted by the PT2001 GDI injector driver IC. The firmware checks:

```
Injection duration excess PT2001 limits time: %.4f
if (isGdiEngine() && isGDIDriverInjectorTimeTooLong)
```

If this condition is true, the injection event is suppressed for that cycle. This is not a tuning-only issue — it is an actual hardware limit of the PT2001 driver.

**Cause in this case**: Fuel delivery parameters were set too high. A reported duration of ~9013 µs was triggering this cut.

**Important**: Reducing the VE table value does **not** affect cranking injection pulse width. Cranking fuel is controlled by the dedicated cranking fuel enrichment setting, not the VE map. Attempting to reduce injection duration by lowering VE to extreme values (e.g., 10%) or global fuel correction to near-zero (0.0001) will not help during cranking.

**Correct approach**:
1. Reduce the **cranking fuel** enrichment setting directly.
2. Disable the **prime pulse** (injector prime on power-up) if it is also triggering the error — prime pulse is a fixed-width injection that also goes through the PT2001 driver.
3. Check that the base injector size/flow rate is not massively over-specified relative to the actual injectors fitted.

### Error Code P06118 — Crank Position Sensor (Transient)

P06118 indicates a crank position sensor signal anomaly. On this project, ggurov confirmed that this code is **transient** — it appears during initial startup attempts and clears on its own once the engine begins running. It does not indicate a persistent crank sensor fault and can be disregarded during early bringup.

### Injector Fire Counter Monitor

To verify which injectors are actually firing (vs being suppressed by the PT2001 duration limit), use the TunerStudio monitor and watch the following variables:

```
injections1 2 3 4 5 6 7 8
```

These counters increment each time the respective injector output fires. If the counter for a given injector is stuck at zero while the engine is cranking, that injector is not firing — either due to the duration cut, a wiring fault, or a driver fault. This is more reliable than a scope check when multiple injectors are involved.

### Also Noted: Injector 1 Going to 12V

A separate symptom observed: Injector 1 (Inj1) output going to 12V during start attempt rather than pulling to ground. This indicates the injector output is not being driven low — possible causes include the duration cut suppressing the event, a wiring fault (open ground), or a driver channel fault. Use the injection counter monitor to differentiate firmware suppression from a hardware failure.

### Summary of Diagnostic Steps for GDI Injection Errors

1. Check if code 9013 is active — if yes, injection duration is exceeding PT2001 limits.
2. Reduce cranking fuel setting (not VE) to bring pulse width within limits.
3. Disable prime pulse to eliminate it as a source of the error.
4. Monitor `injections1`–`injections8` counters to confirm which injectors are firing.
5. P06118 on initial startup is transient and normal — ignore it during bringup.
