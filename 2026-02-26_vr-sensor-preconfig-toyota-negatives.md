# VR Sensor Preconfiguration and Toyota Negative Wiring Notes

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov*

## Summary

These are board-level configuration notes from ggurov covering default VR sensor settings and Toyota-specific negative wiring on the epicEFI/rusEFI board. The crank and cam VR sensors have specific preconfigured defaults, and Toyota vehicles require special attention for the second ground wire because Toyota merges multiple chassis negatives into a single wire.

## Details

### VR Sensor Default Settings

When preconfiguring both VR (Variable Reluctance) sensors on the board, use the following defaults:

- **Crank sensor**: set to `max`
- **Cam sensor**: set to `dsc`

### Toyota Negative Wiring

Toyota vehicles merge multiple chassis negatives into a single wire. The board has a dedicated spot for negative connections. When installing in a Toyota:

- There will be a negative terminal location on the board
- Because Toyota consolidates grounds, you must choose where to route the **second negative wire**
- Users have flexibility in selecting the physical routing location for the additional ground

### Sensor Pin Identification

The board breaks out wires to known position connectors specifically intended for sensor hookup. When identifying pins for sensor connections, refer to the two known-position wires that are broken out to the board header — these are the dedicated sensor connection points.
