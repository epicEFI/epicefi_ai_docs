# Per-Runner Intake Pressure Measurement: Methods and Limitations

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111 (advanced-tuning)*
*Contributors: @Joesphan, @Jordan, @CarolinaCumberland*

## Summary

A technical discussion about whether and how to measure manifold pressure at individual intake runners rather than using a single shared MAP sensor. The conversation covers professional-grade solutions (AVL pressure transducers), DIY approaches (barometric packages), sensor response time requirements, and the fundamental challenge of aliasing when sampling dynamic pressure waveforms with slow sensors.

## Details

### Why Per-Runner Pressure Measurement?

A single MAP sensor shared across all cylinders provides an averaged or manifold plenum pressure reading. For applications where cylinder-to-cylinder variation is important — such as individual runner tuning, ITBs, or high-resolution fuel and ignition correction — a localized pressure reading per runner is desirable.

### Professional Solution: AVL Pressure Transducers

@Jordan described the professional approach used in engine development labs:
> "We use the AVL system, the sensors use 3/8npt ports welded to the intake tubes."

Key requirements for proper dynamic intake pressure measurement:
- **High-speed pressure transducers** are required — approximately one sample per degree of crank rotation for meaningful waveform capture
- The AVL system uses sensors with 3/8 NPT ports welded directly to intake tubes
- These sensors are expensive (approximately $3,000+ each for non-liquid-cooled variants)

### Sensor Types: Piezoresistive vs. Piezoelectric

The AVL sensors have a special interface box. Based on the absence of a charge amplifier in the interface box (charge amplifiers are needed for piezoelectric sensors that produce very small pC outputs), the sensors are inferred to be **piezoresistive** rather than piezoelectric.

### OEM TMAP / MAP Sensor Response Time Limitations

Standard OEM-type MAP/TMAP sensors have approximately **1 ms response time** (e.g., Bosch PST3, MPX4250, MPXH). At engine speeds, 1 ms represents significant crank rotation:

- At 6000 RPM: 1 revolution = 10 ms → 1 ms = 36 degrees of crank rotation

This means standard MAP sensors **cannot capture the dynamic pressure waveform** within an intake runner. However, they can still be useful if sampled at a consistent crank angle (once per cycle), allowing slow compensation for cylinder pressure differences rather than true waveform capture.

@Jordan noted that an earlier attempt to use standard TMAPs in a specific runner for phase information failed completely compared to the AVL system:
> "It was very clear on the AVL system and it was a complete guess with the TMAP."

### DIY Approach: Barometric Packages Snaked into Runner

@Joesphan suggested a lower-cost DIY option:
> "There are packages used for a barometer that can measure pressure, that could be snaked down the runner and glued on."

Small barometric sensor packages (all-in-one with digital output) could potentially be inserted into an intake runner and secured with adhesive. These may have similar or worse response times than standard MAP sensors (~1 ms), limiting them to slow/average pressure measurement rather than dynamic waveform capture.

### Aliasing Concerns

@Jordan warned about significant aliasing when using slow sensors to sample dynamic pressure waveforms:
> "Though you could have significant aliasing."

If the sensor response time is much slower than the pressure wave frequency in the intake runner, the sampled values may not accurately represent the actual pressure wave. However, if only relative or average readings are needed (rather than absolute waveform fidelity), aliasing may be acceptable. Sampling at a fixed crank angle every cycle allows gradual development of the waveform picture over multiple cycles.

### Strain Gauge as Alternative

@Jordan also mentioned strain gauges as a low-cost alternative for detecting pressure waveforms (not absolute values):
> "You could strain gauge something maybe? It will drift a lot with temperature but if you don't care about the absolute values as much as the waveform it could work. You could also compensate if you measure the temp of the gauge."

A Wheatstone bridge strain gauge applied to an intake component could detect deflection caused by pressure waves. Limitations:
- Significant temperature drift
- Not suitable for absolute pressure measurement
- Temperature compensation is possible by measuring the gauge temperature
- Cheap and fast compared to proper pressure transducers; accuracy is sacrificed

As @Joesphan summarized:
> "It's not an option on the triangle: cheap, fast, accurate — pick 2."
