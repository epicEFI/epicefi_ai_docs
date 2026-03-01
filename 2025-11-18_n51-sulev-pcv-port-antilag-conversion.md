# N51 SULEV Head PCV Port Conversion for Anti-Lag / Fresh Air Injection

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312 (theory/dev)*
*Contributors: @fast335xi, @lysdex, @joesphan*

## Summary

Brief discussion of converting the stock PCV (Positive Crankcase Ventilation) head ports on an N51 SULEV (Super Ultra Low Emission Vehicle) head to support fresh-air anti-lag injection, and a broader conversation about rally-style anti-lag strategies.

## Details

### N51 SULEV PCV Port Conversion

The BMW N51 SULEV engine head has stock PCV ports that can be repurposed:

- Add a solenoid to the existing stock PCV head ports
- This allows the ports to be used for fresh-air injection into the exhaust ports (for anti-lag / exhaust turbine spooling)
- @fast335xi: "On the N51 SULEV head you can convert the stock PCV head ports to run it — just add a solenoid"

### Anti-Lag Strategy Discussion

Several anti-lag approaches were discussed:

1. **Fresh air valve injection into exhaust**: Rally-style approach — injects fresh air directly into the exhaust ports to keep the turbine spinning during lift-off. The N51 SULEV PCV conversion enables this on that platform.

2. **Open throttle body + ignition retard**: Open the throttle slightly during anti-lag to maintain airflow, and retard ignition timing to dump unburned energy into the exhaust. This is the more common ECU-only approach.

3. **Rocket anti-lag**: A more exotic variant noted by @lysdex as "not majorly better, just different"

@lysdex: "I don't think there is a huge advantage of fresh air valves on the exhaust vs throttle body open"

The conclusion was that having all the options available is valuable even if the simpler approaches work nearly as well. rusEFI/epicEFI already supports the throttle body + retard approach through existing TB modifier and ignition skip/retard controls.

### Related: Cold Start Injector Discussion

@lysdex raised the idea of supporting cold start injectors (as used on the Toyota 3S-GTE), which inject extra fuel during cold cranking. The conclusion was this is largely unnecessary since modern ECUs (including rusEFI) can simply inject more fuel through the normal injectors during cold start — making a dedicated cold start injector redundant.
