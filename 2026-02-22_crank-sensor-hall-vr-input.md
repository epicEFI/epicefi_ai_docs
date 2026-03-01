# Crank Sensor Input Selection: Hall vs VR in epicEFI

*Source: epicEFI Discord, 2026-02-22 | Channel: 1356732771325968630*
*Contributors: @offtheband, @ggurov*

## Summary

A user was unsure which input to use for their crank sensor in epicEFI because there was no input explicitly labelled "crank sensor." The answer is to identify the sensor type (Hall-effect or VR/variable reluctance) and wire it to the corresponding typed input. Available inputs on the tested board were: Flex, Hall, Hall, Hall, VR, VR.

## Details

### Finding the Crank Input

epicEFI does not have a single input labelled "Crank Input." Instead, inputs are labelled by their electrical type:
- **Hall-effect sensors** (digital, square wave output) — use a **Hall** input
- **VR sensors** (variable reluctance, sine wave output) — use a **VR** input

The user's board exposed the following sensor inputs:
- Flex (hall-compatible)
- Hall × 3
- VR × 2

### Configuration Steps

1. Identify whether your crank sensor is Hall-effect or VR (check sensor documentation or OEM specs)
2. Wire the sensor to the appropriate typed input (Hall or VR)
3. In the software, the crank trigger input is assigned in the trigger/wheel decoder configuration — select the physical pin that corresponds to the input you wired to

### Example (this session)

User confirmed their crank sensor is a **Hall-effect sensor**. ggurov advised:
- "pick hall" — select one of the Hall inputs
- "wire it into that"

### VSS Note

The user also asked about Vehicle Speed Sensor (VSS). VSS is a separate input and can be wired up independently as needed — it does not compete with crank input assignment.
