# Electric Power Steering CAN Dongle Integration (Ford Fiesta)

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @mke, @YeOldePirate, @Joesphan*

## Summary

Discussion about using an aftermarket CAN dongle from eBay to control an electric power steering (EPS) unit from a Ford Fiesta in a custom/swapped application. The dongle provides three torque settings and communicates via CAN bus. Key consideration is that the EPS unit requires a live CAN signal to function — it will not operate in a purely passive mode.

## Details

### Hardware

- A CAN dongle sourced from eBay (~$50) is pre-programmed with Ford Fiesta EPS CAN codes.
- The accompanying EPS unit itself was approximately $104.
- The dongle exposes three torque settings, which can be mapped to vehicle speed for OEM-like behavior.

### CAN Signal Requirement

The Ford Fiesta EPS unit **requires a CAN signal to operate**. It cannot be run without one. This is a critical constraint when adapting the unit to a non-Fiesta vehicle.

### Alternative Approaches

- The Prius EPS module was discussed as a fallback option since Prius CAN codes have been published online. The Prius module can reportedly also operate without CAN in certain configurations.
- The MR2 Spyder EPS unit was mentioned as another option that works without CAN.
- For custom applications, Lua scripting within rusEFI could potentially generate the required CAN messages to control the EPS unit, allowing the rusEFI ECU to replace the dongle entirely.

### Speed-Based Torque Mapping

One contributor noted a plan to vary the torque setting based on vehicle speed, replicating OEM variable-assist steering behavior using the three available torque settings and a lookup based on speed.
