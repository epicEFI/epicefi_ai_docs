# Autotune RPM Wall and C9012/C6899 Console Warnings

*Source: epicEFI Discord, 2026-02-24 | Channel: 1356732771325968630*
*Contributors: @Immrpablo, @ggurov*

## Summary

A user running rusEFI autotune encountered an RPM ceiling of approximately 4800 RPM despite the engine being capable of 7000+ RPM with a less aggressive tune. The root cause was an excessively rich VE table causing spark blowout at higher RPM. Separately, the user was seeing random C9012 and C6899 console warnings, which were diagnosed as likely misfire-related events triggered by the same over-fueling condition.

## Details

### Symptom: RPM Wall During Autotune

- Engine would hit a wall at approximately **4800 RPM** and not rev higher, as if a two-step or rev limiter was active.
- Loading a less aggressive tune allowed the engine to rev freely to **7000+ RPM**.
- The only differences between tunes were VE table adjustments, AFR table, and ignition timing.
- No fuel cuts or timing cuts were visible in the logs.

### Root Cause: Extreme Over-Fueling (Spark Blowout)

The mixture was so rich that it was **blowing out the spark** at higher RPM. This is a known phenomenon where an excessively rich mixture prevents reliable combustion above a certain RPM threshold, creating what appears to be a soft rev limiter.

- @ggurov's diagnosis: "it's not just rich, it's rockefeller rich"
- Confirmed by @ggurov: the spark was being blown out by the excessively rich mixture.

### Resolution

The user planned to reduce VE table values by approximately **20%** globally to bring the mixture into a range where combustion is reliable at higher RPM before continuing autotune.

### Console Warnings: C9012 and C6899

The user also reported intermittent C9012 and C6899 warnings appearing in the TunerStudio or rusEFI console.

- **Probable cause**: These warnings are likely misfire-detection events. When the engine is running significantly richer than the predicted AFR, combustion events become inconsistent and the ECU detects that the engine is not accelerating (or behaving) as predicted.
- @ggurov: "prolly misfires, the engine isn't accelerating like it's predicted -- would be my guess"
- The user confirmed that a trigger log at idle looked normal, ruling out a trigger wiring/signal issue as the source.
- The warnings were likely a symptom of the over-fueling rather than an independent wiring problem.

### Recommendations

1. Before running autotune, ensure the base VE table is not severely over-rich -- autotune works best when starting close to target AFR.
2. If RPM hits a wall with no logged cuts, suspect spark blowout from over-fueling.
3. C9012 and C6899 warnings in the context of a rich tune are likely misfire indicators, not wiring faults. Resolve the fueling issue first before investigating wiring.
4. Trigger log at idle being clean rules out sensor/wiring issues for those codes.
