# Programmable Outputs for Air Shift Solenoid and External Device Control

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312*
*Contributors: @benassi125, @ggurov*

## Summary

A user asked how to configure conditions for triggering an air shift solenoid. The guidance was to use the programmable outputs feature in rusEFI/epicEFI, which supports up to three simultaneous conditions for activating an external output. This feature is the correct mechanism for controlling external devices such as solenoids based on engine parameters.

## Details

### Use Case: Air Shift Solenoid

The user needed to trigger an air shift solenoid based on specific engine operating conditions (e.g., RPM threshold, load, or speed).

> "to set conditions for when I want the actual airshift solenoid to be triggered"
> — @benassi125

### Programmable Outputs Feature

rusEFI/epicEFI has a programmable outputs system designed for exactly this type of external device control. Each programmable output supports up to three conditions that must all be satisfied before the output is activated.

> "i've got a place to look at, we have programmable outputs for external stuff with 3 conditions"
> — @ggurov

### Configuration Approach

- Navigate to the programmable outputs section in the rusEFI/epicEFI configuration interface
- Assign the output pin connected to the solenoid
- Define up to three conditions based on engine parameters (such as RPM range, MAP/load range, TPS position, vehicle speed, etc.)
- The output activates when all configured conditions are simultaneously met

This provides a flexible, no-Lua-script approach for controlling ancillary solenoids, relays, and other actuators based on engine state.
