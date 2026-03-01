# DI Fuel System Sensor Configuration: Rail Sensor Input, LPFP/HPFP Setup, and Pressure Monitoring

*Source: rusEFI Discord, 2026-02-23 | Channel: 1356732771325968630*
*Contributors: @offtheband, @joesphan, @ggurov*

## Summary

A user configuring a direct injection (DI) fuel system asked about locating the fuel rail sensor input setting in rusEFI/epicEFI, the relationship between the low-pressure fuel pump (LPFP) and high-pressure fuel pump (HPFP) configurations, and how to prioritize sensor data when diagnosing fuel system issues. Key clarifications were provided on where the sensor settings live in the UI, how bypassing the LPFP affects available configuration options, and the importance of rail pressure as the primary diagnostic signal for DI.

## Details

### Locating the Fuel Rail Sensor Input Setting

The fuel rail sensor input setting is found under the **Injection** section of the rusEFI/epicEFI configuration UI. Users who perform a search and land on an unfamiliar page should navigate there directly via the Injection menu rather than relying on the search result landing page.

- Search term to use: **"low pressure"**
- UI location: **Injection** section

### LPFP vs. HPFP Configuration — Bypassing the Low-Pressure Pump

When "channeling out the rail" (i.e., running fuel directly to the high-pressure rail without using an intermediate low-pressure fuel pump stage), the LPFP-specific configuration fields will not apply. The concern raised was whether bypassing the LPFP would cause important settings to be missed.

- If the LPFP is bypassed or not present in the system, the LPFP-related input/sensor configuration pages will not be relevant to the setup.
- The HPFP remains the primary pump for DI rail control in this configuration.

### HPFP Target Rail Pressure

The DI rail pressure is controlled to the HPFP target with a tolerance band:

- **Rail pressure setpoint:** HPFP target delivered **± 3000 psi**

This means the actual rail pressure will seat within 3000 psi of the HPFP commanded target pressure.

### Monitoring Low Fuel Pump Pressure

Even when running a DI-only (HPFP-primary) configuration, the low-side pump pressure still needs to be monitored to ensure the HPFP is receiving adequate supply pressure. A dedicated sensor and corresponding rusEFI input configuration is required for this.

### Diagnostic Priority: Rail Pressure vs. Low Pump Data

When diagnosing DI fuel system issues, the approach is divide-and-conquer across two data sources:

1. **Fuel rail pressure** — the primary diagnostic signal for DI systems. Rail pressure directly reflects whether the HPFP is delivering correctly.
2. **Low pump pressure** — secondary signal used to determine if supply-side starvation is causing the rail pressure problem.

Recommendation: prioritize **fuel rail pressure data** first when diagnosing DI issues, then use low pump pressure data to identify root cause if rail pressure is abnormal.
