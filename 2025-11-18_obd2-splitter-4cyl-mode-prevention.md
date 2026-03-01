# OBD2 Splitter Requirement to Prevent 4-Cylinder Mode

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312 (theory/dev)*
*Contributors: @joesphan, @ggurov*

## Summary

@joesphan noted a requirement to use an OBD2 splitter on his vehicle to prevent the engine management system from entering 4-cylinder mode (cylinder deactivation). The context is using an OBD2 data logger (such as Torque Pro) alongside the stock ECU or an aftermarket ECU that monitors OBD2 traffic.

## Details

### The Problem

When connecting an OBD2 data logger or diagnostic tool to a vehicle that supports cylinder deactivation, the OBD2 activity on the bus can sometimes trigger the ECU to switch into 4-cylinder (deactivated) mode. This is caused by the OBD2 tool's polling interfering with the engine control CAN bus messages.

@joesphan: "need to get an OBD2 splitter though so it doesn't go into 4-cylinder mode"

### Solution: OBD2 Splitter

An OBD2 splitter (Y-cable) allows both the stock ECU communication and a separate diagnostic/logging device to access the OBD2 port simultaneously without the diagnostic device's requests appearing as ECU commands on the main powertrain CAN bus.

### Context: Torque Pro Logging

@joesphan was planning to use Torque Pro to log MAF values on his truck (a pushrod V8 with cylinder deactivation). @ggurov asked whether the "RT" suffix in Torque Pro refers to real-time data, which @joesphan confirmed he wasn't certain about.

### Related: CAN Sniffing for BMW ECU Data

@fast335xi noted that for BMW vehicles, live tuning is possible via Vehical and compatible tune editors on newer models. A repository of BMW XDF files (including S55 and B58 maps) is available at:
https://github.com/dmacpro91/BMW-XDFs
