# Ignition System: Coil Minimum Pulse Width, Waste Spark Wiring, and Component Quality

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111 (advanced-tuning)*
*Contributors: @Chemex-joshseek, @DCwerx, @MykkH, @ggurov*

## Summary

A set of related observations about ignition coil behavior and wiring best practices. Key topics include the minimum acceptable coil dwell (pulse width) for Toyota coils, the importance of not using cheap coils or injectors, and wiring recommendations for waste spark setups using separate fused circuits per coil bank.

## Details

### Ignition Event Monitoring

rusEFI provides a signal that activates to 1 when an ignition event occurs, along with a counter that increments for each cylinder during ignition and injection events. These can be used for diagnostics to confirm actual spark and injection events are taking place per cylinder.

### Toyota Coil Minimum Pulse Width

Toyota ignition coils have a minimum required dwell time of approximately **2 ms**. If the commanded pulse width is shorter than this threshold, the system will default to approximately **1.8–2 ms**. Attempting to fire the coil below this minimum risks incomplete charging and missed or weak spark events.

This is relevant when tuning dwell tables at high RPM or low loads where calculated dwell may become very short.

### Component Quality for Coils and Injectors

@DCwerx:
> "Never cheap out on coils, injectors. I will risk a China FPR if my car has a good AFR safety. The important bits like injectors and coils I won't chance it."

Critical components where quality should not be compromised:
- **Ignition coils** — cheap coils can cause misfires, inconsistent dwell, or outright failure
- **Fuel injectors** — cheap injectors may have inconsistent flow rates, dead times, or seal failures

Lower-stakes components (e.g., fuel pressure regulators) may be acceptable in budget form if a safety system (AFR monitoring/cut) is in place.

### Waste Spark Wiring: Separate Fused Circuits per Bank

For waste spark ignition systems, it is good practice to run separate fused circuits for each coil bank. @MykkH:
> "I run B1 & B2 +12v from different fused circuits because waste spark"

- **B1 coils** receive +12V from one fused circuit
- **B2 coils** receive +12V from a separate fused circuit

This provides independent protection for each bank and prevents a single fuse failure from killing all ignition, and also limits fault propagation if one bank develops a short.
