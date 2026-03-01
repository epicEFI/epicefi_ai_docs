# Exhaust Gas Temperature (EGT) Normal Ranges for N/A Engines

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111 (advanced-tuning)*
*Contributors: @kostasl, @Joesphan, @Jordan*

## Summary

A brief discussion on what constitutes normal exhaust gas temperatures (EGTs) for a naturally aspirated engine at wide open throttle (WOT). The consensus is that EGTs are highly variable and depend on many factors, making absolute "normal" values difficult to define. EGT sensors are most useful for cylinder balance checks and, on turbocharged engines, for monitoring turbine inlet temperature limits.

## Details

### EGTs Are Highly Variable

There is no single "normal" EGT value for an N/A engine at WOT. EGTs are affected by:

- **Ignition timing** — more advance generally means lower EGTs (more energy converted to work)
- **Air-fuel ratio** — richer mixtures tend to lower EGTs; lean mixtures raise them
- **Engine load and RPM**
- **Displacement, compression ratio, and combustion chamber geometry**
- **Fuel type**

### Practical Use of EGT Sensors

@Jordan:
> "I would only rely on it to check cylinder balance but you have to do mixture sweeps and see where each cylinder peaks. You can also use one in the collected on a turbo to make sure you're not exceeding turbine limitations."

Primary use cases for EGT sensors:

1. **Cylinder balance**: By comparing EGTs across cylinders and performing mixture sweeps, you can identify cylinders running rich or lean relative to others, which may indicate injector issues, air distribution problems, or cam/valve problems.

2. **Turbocharged engines**: A pre-turbine EGT sensor is valuable for ensuring turbine inlet temperatures remain within the turbocharger's rated limits, particularly at sustained high boost/load conditions.

For N/A engines, EGT sensors are useful diagnostic tools but should not be used as the sole metric for determining if a tune is safe or optimal. AFR and ignition timing data provide more reliable tuning targets.
