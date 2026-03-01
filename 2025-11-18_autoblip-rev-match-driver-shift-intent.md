# Autoblip / Rev-Match Feature: Driver Shift Intent, Sensor Requirements, and Telemetry Approach

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @ggurov, @walterronny, @lysdex*

## Summary

This thread discusses the implementation of an autoblip (rev-match on downshift) feature in rusEFI/epicEFI. The conversation covers the sensor inputs required for detecting driver shift intent (DBW throttle, clutch pressure sensor, brake pressure sensor/switch), an alternative RPM-threshold-based heuristic for triggering blips, and a telemetry-driven approach where logged data is used to determine after the fact whether a blip was needed, to inform calibration.

## Details

### Autoblip Feature — Current State

@Walter R. noted that autoblip functionality is rare in aftermarket EMS:

> "autoblip. i think only haltech has that feature working."

Autoblip (also called rev-match) automatically blips the throttle during a downshift to synchronize engine speed with the lower gear's expected RPM, preventing driveline shock and wheel lockup.

### Driver Shift Intent — Required Inputs

@ggurov identified that implementing autoblip requires detection of *driver shift intent* — the ECU must know a downshift is being initiated before it can execute a blip. @Walter R. clarified what sensor inputs are needed:

> "it works with DBW, clutch press sensor, brake press sensor/switch and it's done."

Required sensor inputs:
- **DBW (Drive-By-Wire) throttle** — allows the ECU to actuate throttle independently of the driver's pedal input
- **Clutch pressure sensor** — detects clutch pedal actuation (indicating a gear change is in progress)
- **Brake pressure sensor or switch** — detects braking (downshift under braking is the primary autoblip use case)

All three inputs together allow the ECU to infer that the driver is performing a braking downshift and to execute an appropriate throttle blip.

### RPM-Threshold Heuristic for Downshift Detection

@Lysdex proposed a simpler heuristic as an alternative or complement to the full sensor approach:

> "If below x rpm assume downshift and blips"

In this approach, if the engine RPM drops below a configurable threshold (relative to the current gear's expected cruising range), the ECU assumes a downshift is happening and triggers a blip. This is less precise than using a clutch sensor but can work in simpler installations.

### Telemetry-Driven Calibration Approach

@ggurov outlined a data-driven methodology for determining when and how much to blip:

> "we'll work on proper telemetry and just have the data tell us when a blip was needed"

Rather than hard-coding blip timing and magnitude up front, logged telemetry from real driving is analyzed to identify events where a blip would have improved drivability. This iterative approach lets calibration be informed by real-world data rather than assumptions, and is especially useful during initial feature development before the feature is fully validated.

### Implementation Notes

- DBW is a prerequisite — without electronic throttle control, the ECU cannot execute a blip independently
- Clutch pressure sensor is preferred over a simple on/off clutch switch for more precise timing
- Brake pressure (not just a switch) allows the ECU to distinguish light braking from hard braking and scale the blip accordingly
- The telemetry approach is planned as a development/calibration aid to validate blip strategy before releasing as a user-facing feature
