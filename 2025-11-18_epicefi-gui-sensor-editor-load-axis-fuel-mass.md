# epicEFI Tuning GUI Development: Sensor Editor, Load Axis Table, and Fuel Mass Calculation

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @mitch0s, @ggurov*

## Summary

This thread covers active development work on the epicEFI tuning software GUI, including the sensor creation and editing interface, the load axis table implementation, sensor readout display fields (16-bit raw, voltage, scaled value), and the need to route tuning output through the standard fuel mass calculation pipeline. The developer (@mitch0s) is also setting up a G4ED engine benchtest as the first real hardware platform for EMS feature development and tuning.

## Details

### Sensor Creation and Editing Interface

@mitch0s was working on the sensor creation and editing feature in the GUI:

> "gonna work on sensor creation and editing today. How's this look?"

The interface was shared as an image for review. The planned functionality covers adding new sensor types, configuring existing sensors, and verifying sensor data accuracy.

### Load Axis Table and Sensor Readout Display

Alongside the sensor editor, the load axis table UI was in progress. The planned sensor readout panel would display three values per sensor:

> "Just got to make the load axis table thing, as well as maybe the sensor readouts, like the 16bit reading, voltage reading, and value reading on the side"

- **16-bit reading** — the raw ADC count from the sensor
- **Voltage reading** — the converted voltage value
- **Value reading** — the final scaled/interpreted sensor value (e.g. kPa, °C, etc.)

### Fuel Mass Calculation on Output

@ggurov noted that the output path should eventually pass through the same fuel mass calculation used in production EMS code:

> "should end up going through eventually the same fuel mass calculation on output"

@mitch0s agreed, noting that proper fuel mass calculation is needed once a real engine is available for tuning:

> "yeah 100%. I also still need to get that G4ED benchtest setup going too. That will be the first engine I *properly* tune, and use to develop features. Until then, I'm sort of just having to guess what would be needed/required by an EMS"

### G4ED Benchtest Setup

The G4ED engine is planned as the first real hardware benchtest for feature development. Until a running engine is available, EMS feature requirements must be estimated. The benchtest will:
- Enable proper closed-loop tuning verification
- Serve as the reference platform for developing and testing EMS features going forward

### TunerStudio Replacement Question

A community member asked about alternatives to TunerStudio:

> "Replacement for tuner studio?"

The context of these threads strongly suggests the epicEFI GUI under development (described above) is intended to serve this role — a native GUI for rusEFI/epicEFI hardware, replacing the dependency on the third-party TunerStudio application.
