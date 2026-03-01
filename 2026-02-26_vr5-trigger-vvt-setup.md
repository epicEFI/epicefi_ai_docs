# VR5 Engine Trigger Wheel and VVT Cam Sensor Configuration

*Source: rusEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @Bearded_Bogan, @audigates*

## Summary

Brief discussion of trigger wheel and VVT cam sensor configuration for a VR5 engine. The engine was initially fired on a subset of cylinders with the trigger system not yet configured. Crank and cam trigger types were identified for a VW VR5 application.

## Details

### VR5 Engine Trigger Configuration

- **Crank trigger wheel:** 60-2 tooth wheel (60 teeth minus 2, standard missing-tooth pattern)
- **Cam sensors:** Two Bosch cam sensors, one per camshaft, used for VVT (Variable Valve Timing) control on both intake and exhaust cams

### Status During Commissioning

The engine was initially started and running on a partial set of cylinders before trigger inputs were fully configured in rusEFI. This is a common bring-up sequence — the engine can fire on coil-on-plug or wasted spark from a basic sync before full sequential control is established.

### Notes

- A VR5 engine uses a single VR (variable reluctance) crank sensor reading the 60-2 wheel for primary engine position
- Both camshaft positions are tracked via Hall-effect or VR-type Bosch sensors for dual-VVT operation
- Confirm trigger wheel tooth count and sensor type in TunerStudio before attempting sequential fuel/ignition
