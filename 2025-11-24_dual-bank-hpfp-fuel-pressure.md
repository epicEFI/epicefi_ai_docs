# Dual-Bank HPFP Control and Dual Rail Pressure Sensing — Feature Requirements

*Source: rusEFI Discord, 2025-11-24 | Channel: 1442636693474902148*
*Contributors: ggurov (@ggurov), Tera (@teraflopping)*

## Summary

A design discussion around extending rusEFI/epicEFI to support two independent high-pressure fuel pump (HPFP) control banks and two independent fuel rail pressure sensors. The conversation covers the required changes across pump configuration, sensor inputs, VVT cam association, closed-loop control, and dashboard gauges.

## Details

**Motivation — second fuel rail and injector correction:**
The discussion was prompted by a use case involving a second high-pressure fuel rail. ggurov raised the need to be able to correct injectors on a specific (second) fuel rail using its own pressure reading, and asked what debug and calculation data should be exposed for this.

**Closed-loop HPFP control:**
An existing HPFP target pressure map is already present in the firmware. The requirement is for that closed-loop control to support splitting into two independent banks — one per fuel rail.

- Existing: HPFP target pressure map.
- Required: closed-loop control must be split into two banks.

**Pump config menu — second HPFP output pin:**
To drive two HPFPs independently the pump configuration menu must expose a second HPFP output pin assignment, one per bank.

- Required addition: second HPFP output pin in the pump config menu for the second engine bank.
- Required addition: second bench-test button for the second HPFP (for priming / diagnostics).

**VVT cam association for HPFP drive:**
When configuring which cam drives the HPFP (needed for lobe-based pump timing), Tera noted that since VVT already runs in closed loop, the association does not need a numeric cam index. The selection can be simplified to:

- `Exhaust` — pump is driven from the exhaust cam.
- `Intake` — pump is driven from the intake cam.

Location: Cam config / VVT settings.

**Fuel pressure sensor — second input:**
A second fuel rail pressure sensor input must be added so both rails can be monitored independently.

- Location: `Sensor > Fuel Sensors > Fuel Pressure`
- Required: add a second rail pressure input, enabling two-bank fuel pressure sensing.

**Dashboard gauges:**
For runtime monitoring, a second fuel pressure gauge should be added to the dashboard alongside the existing one, so both rail pressures are visible in real time.

**Injector bank selection (existing, purpose queried):**
Tera noted that the injector hardware menu already contains a bank selection option. Its purpose in the context of multi-rail operation was queried. ggurov's response was that it is used for wideband (lambda sensor) bank assignment, not for fuel delivery bank splitting directly. This may be reusable for the dual-bank rail pressure correction logic.
