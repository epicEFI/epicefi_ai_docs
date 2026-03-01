# MAF-Less Airload Measurement, EFI Carb Conversion, and Standalone ECU Integration

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @kostasl0737, @joshseek_30236_07984 (Chemex-joshseek), @carolinacumberland*

## Summary

Multiple related threads discussed alternatives to MAF sensor-based air measurement, the challenges of EFI conversions on carbureted engines using standalone ECUs and non-OEM harnesses, and a concept for an all-in-one EFI throttle body unit for carb-to-EFI conversions. The common theme is integrating EFI systems into vehicles that were not originally equipped with MAF sensors or modern EFI.

## Details

### Running Without a MAF Sensor (MAP + VE Table)

When a customer or builder refuses to use a Mass Air Flow (MAF) sensor, the standard alternative is a MAP sensor combined with a volumetric efficiency (VE) table:

> "Looks like you forgot maf" — @Chemex-joshseek
> "Customer won't use maf" — @kostasl0737

The MAP + VE (speed-density) approach:
- Uses manifold absolute pressure and engine speed (RPM) to estimate air mass entering the cylinder
- Relies on a calibrated VE table that characterizes the engine's breathing efficiency across the RPM and load range
- Does not require an air mass sensor in the intake tract
- Requires more careful calibration than MAF-based systems but is widely used and fully supported in rusEFI/epicEFI

### Standalone ECU Integration with Factory Harness

When converting a non-original engine into a vehicle with factory chassis and dash harnesses, the ECU harness is typically a separate standalone unit:

> "It is deeper than that though, like my truck, I have the factory chassis harness and dash harness for lights, but my ECU is a standalone harness since the motor is not original to my truck" — @CarolinaCumberland

For carbureted vehicles with minimal original sensors:

> "Also my truck had almost zero sensors since it was a carb motor. I did use the factory harness that came with my motor for parts, connectors and wires." — @CarolinaCumberland

Practical guidance:
- Keep the factory chassis harness for lighting, dash functions, and body electronics
- Build or use a standalone engine harness for the ECU, matched to the specific engine being installed
- Reuse connectors and wire sections from the engine's original harness where possible to ensure proper connector fitment

### EFI Carb Conversion Using CBR600 Throttle Bodies and Speeduino

A community member helped direct a builder wanting to convert a Toyota 22R carbureted motor to EFI using Honda CBR600 throttle bodies and the Speeduino ECU:

> "I just tagged you and [speeduino experts] in general on speeduino to help a guy that wants to convert a 22r carb motor to efi using cbr600 efi throttle bodies and speeduino" — @CarolinaCumberland

Notes on the 22R conversion context:
- The 22R has quirks that make Speeduino integration non-trivial
- CBR600 throttle bodies (individual throttle bodies, ITBs) require careful idle and TPS calibration
- The conversation implied rusEFI/epicEFI was not chosen for this specific conversion due to the 22R's complexity

### Integrated EFI-Carburetor / TBI Unit Concept

An idea was raised for a combined unit that could bolt onto standard carburetor mounting patterns:

> "I've been thinking about recreating a EFI carb or tbi, but it's injector, throttle body, tps, map sensor and iat all bundled in one that has several popular carburetor bolt patterns on it." — @Chemex-joshseek

Proposed integrated unit would include:
- Fuel injector(s)
- Throttle body
- Throttle Position Sensor (TPS)
- MAP sensor
- Intake Air Temperature (IAT) sensor
- Multiple carburetor bolt pattern compatibility (e.g., Holley 4150, Rochester, Edelbrock)

This concept is similar to existing TBI (throttle body injection) aftermarket products but with a more complete sensor integration, potentially simplifying carb-to-EFI conversions on classic vehicles.
