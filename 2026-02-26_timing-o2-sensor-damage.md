# Low Ignition Timing Can Damage O2 Sensors

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov, @Ognjen Galic*

## Summary

Excessively retarded ignition timing can cause unburned fuel to reach and damage the oxygen (O2) sensor. The effect can be mitigated by adjusting timing, but care must be taken not to advance too aggressively in the opposite direction.

## Details

### Mechanism

- Running ignition timing **too low (retarded)** causes incomplete combustion, resulting in unburned fuel and hot exhaust gases reaching the O2 sensor.
- This can **fry (destroy) the O2 sensor** — a direct consequence of over-retarded timing.

### Mitigation

- Timing can be used to "tame" rich/hot exhaust conditions — advancing timing slightly helps ensure more complete combustion before exhaust valve opening.
- Balance is required: timing must not be set so low that it damages the O2 sensor, but advancing too much introduces knock risk.

### Practical Note

- This issue is most likely to surface during idle or low-load tuning when timing tables may be set conservatively or incorrectly.
- If an O2 sensor fails prematurely or reads erratically, check ignition timing as a potential root cause before replacing the sensor.
