# TCC Lock/Unlock Control: Adding TPS Input and Improved Shift Point Table Design

*Source: rusEFI Discord, 2026-02-22 | Channel: 1470466409602744541*
*Contributors: @Robb235 (@robb235_00916), @ggurov (@ggurov), @epicEFIdev*

## Summary

User Robb235 identified several limitations in the current epicEFI/rusEFI torque converter clutch (TCC) lock/unlock control and automatic transmission shift point tables. The primary issue is that TCC lock/unlock decisions currently only consider gear and vehicle speed (VSS), with no consideration of driver demand via TPS. Robb235 proposed a redesigned 3D table (TPS x VSS) for both shift points and TCC lock/unlock thresholds, and suggested adding a minimum TPS% threshold parameter to prevent TCC engagement under high throttle. ggurov clarified that the current "min load" / "max load" values refer to "fuel load" (not TPS), and engaged with Robb235 to better understand the use case before reviewing older TCU reference code.

## Details

### Current TCC Lock/Unlock Menu Limitations

- Current inputs for TCC lock/unlock decisions: gear position, VSS (vehicle speed)
- Missing input: driver demand / throttle position (TPS%)
- Risk: TCC may lock at inappropriate times (high throttle, aggressive acceleration) causing transmission stress or driver discomfort

### Proposed TCC Lock/Unlock Table Redesign

Robb235 proposed restructuring the TCC lock/unlock table to mirror the Automatic Shift Points table design:

- **X-axis:** TPS% (throttle position sensor, 0-100%)
- **Y-axis:** gear and function (e.g., 3rd gear lock, 3rd gear unlock, 4th gear lock, 4th gear unlock, etc.)
- **Z-axis:** VSS (vehicle speed sensor)

This 3D structure allows TCC lock thresholds to vary by both speed and driver demand rather than using a single flat speed threshold per gear.

### Proposed Automatic Shift Points Table Redesign

The same design criticism applies to the existing shift points table: current "min load" and "max load" values per gear are flat (not speed-dependent). Robb235 proposed:

- Replace flat per-gear min/max load thresholds with a 3D table
- **X-axis:** TPS%
- **Y-axis:** gear and function (upshift/downshift for each gear)
- **Z-axis:** VSS
- This enables speed-dependent shift points that respond to driver intent

### Clarification: "Fuel Load" vs TPS

ggurov clarified that the existing "min load" / "max load" parameters in the TCC/shift tables refer to **fuel load**, not TPS. This distinction matters for the redesign since fuel load and TPS% are different signals.

### Suggested Parameter Addition: Minimum TPS% for TCC Lock

Robb235 suggested adding a configurable parameter:

- **"Minimum TPS% Required for TCC Lock"**
- Purpose: prevents TCC from locking when the driver is requesting significant throttle (high load / performance driving)
- This is a simpler interim solution compared to the full 3D table redesign

### Use Case Context

- Robb235's primary goal for TCC lockup is **fuel economy** and **transmission temperature reduction** (not performance)
- Transmission temperatures drop significantly when TCC lockup is achieved
- Concern is avoiding excessive power transmission through the TCC at high throttle

### Next Steps (at time of discussion)

- ggurov planned to review his older (12-year-old) TCU reference code to compare how TCC and shift point logic was previously implemented
- ggurov requested user stories / specific scenarios to better understand the desired behavior before redesigning the table
- An `.ini` file was needed for Robb235 to examine the configuration in TunerStudio
