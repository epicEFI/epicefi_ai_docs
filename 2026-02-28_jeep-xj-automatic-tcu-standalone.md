# Jeep XJ Automatic Transmission Running Standalone on epicEFI TCU Code

*Source: epicEFI Discord, 2026-02-28 | Channel: 1356732771325968630*
*Contributors: @Robb235, @ggurov*

## Summary

A 1999 Jeep XJ (turbocharged, previously running uaEFI in piggyback with the stock ECU) was converted to fully standalone operation using the epicEFI TCU (transmission control unit) code. This is a notable validation that the epicEFI TCU code supports automatic transmissions in fully standalone deployments.

## Details

### Vehicle and History

- Vehicle: 1999 Jeep XJ (Cherokee)
- Engine: stock internals, turbocharged (aftermarket)
- Transmission: automatic
- Previous configuration: uaEFI running in piggyback mode with the stock ECU for approximately one year
- New configuration: fully standalone using epicEFI, including TCU code for automatic transmission control

### TCU Code Status

ggurov confirmed the automatic transmission control is "now running on our TCU code," indicating the epicEFI TCU codebase is functional for this application as of late February 2026. The XJ running standalone with the TCU handling the automatic transmission represents a real-world validation of this feature.

### How the User Found epicEFI

The user discovered epicEFI through the DCwerx website, which referenced epicEFI. Searching for epicEFI led to the project wiki. This is relevant for community outreach context.

## Notes

- This is an early real-world report of the epicEFI TCU code running an automatic transmission in a standalone (not piggyback) configuration.
- The supercharger/turbo combination on this XJ uses an Eaton M62 supercharger (borrowed from an early 1990s Pontiac/Buick) running approximately 9 psi on a 2.1" pulley, all on stock internals and factory head gasket. IAT sensor is positioned pre-compressor due to space constraints.
- The IAT correction table was zeroed out in the tune (IAT compensation not active), which happened to work acceptably with the pre-compressor sensor placement.
