# EGT Sensor Selection and Tuning Limitations

*Source: epicEFI Discord, 2026-02-21 | Channel: 1411925674498719775*
*Contributors: @Joesphan, @Jackscr, @tuga702, @digmorepaka*

## Summary

EGT (exhaust gas temperature) sensors are useful for safety monitoring and cylinder balance analysis but cannot be used for primary fuel/AFR tuning. The AFR-vs-EGT curve forms a V or mountain shape, making it impossible to determine whether EGT is high due to lean or rich conditions. Closed-tip thermocouples are safer to install but respond slowly; open-tip thermocouples respond faster but may leak. All K-type thermocouples are broadly equivalent in accuracy.

## Details

### Why EGT Cannot Be Used for Fuel Tuning

- EGT plotted against AFR forms a **"V" or mountain-shaped curve**: both very lean and very rich mixtures produce high EGT, while the stoichiometric zone is cooler.
- This makes EGT **ambiguous** — a high reading could indicate a lean or a rich condition.
- Conclusion: **"you can't tune off EGT"** — it cannot be used to target a specific AFR.

### Valid Uses of EGT

- **Cylinder balance** — comparing EGT between cylinders to identify misfires, injector faults, or spark issues.
- **Safety / overtemp protection** — catching dangerously high exhaust temperatures.
- **VNT turbo control** — monitoring turbine inlet temperature as part of variable-geometry turbo management.
- **Timing retard monitoring** — confirming that knock-induced timing retard is not causing excessive heat.
- **Aircraft/constant-load applications** — EGT is well-suited to steady-state operation (e.g., aircraft engines at cruise), where the curve ambiguity is less of an issue.

### Thermocouple Type: Open vs. Closed Tip

- **Closed-tip thermocouples**: safer (no exhaust gas leak), but **slow response time** — primarily useful for steady-state and safety monitoring, not transient tuning. 5 mm probes are worst; 3 mm are better but still slow for street tuning.
- **Open-tip thermocouples**: faster response, but **may leak exhaust gas** through the sensor body. This can be verified by submerging in water and blowing through — bubbles indicate a leak. Quality varies; some cheap open-tip sensors leak, others do not.
- **Recommendation for street tuning**: if response time matters, open-tip — but verify it doesn't leak. For pure safety monitoring, closed-tip is sufficient.

### K-Type Thermocouple Quality and Cost

- All K-type thermocouples are broadly similar in accuracy — the difference between a 30 EUR and a 300 EUR probe is small.
- Analogy: **"less difference between 30 EUR TC and 300 EUR TC than between a 5 cent and 15 cent resistor"** (digmorepaka).
- Budget K-type thermocouples are generally acceptable for this application.
