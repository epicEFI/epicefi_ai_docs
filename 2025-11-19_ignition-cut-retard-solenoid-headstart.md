# Ignition Cut Retard Output for Solenoid Pre-activation (Gear Change Headstart)
*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov (ggurov), @kostasl (kostasl0737)*

## Summary

Describes how to use the ignition cut retard output signal within the programmable outputs system to pre-activate a solenoid before the ignition cut fires. This is relevant for quick-shift or launch control systems where the solenoid (e.g., a shift cut solenoid) needs a "headstart" to reach full actuation before the ignition cut occurs.

## Details

### Problem

A solenoid controlling a gear change mechanism (e.g., on a sequential gearbox or quick-shifter) requires some lead time to actuate before the ignition cut should fire. If the solenoid and ignition cut are triggered simultaneously, the solenoid may not be fully engaged when the cut happens.

### Solution: Use Ignition Cut Retard Output in Programmable Outputs

rusEFI/epicEFI's **ignition cut retard** function sets an internal output signal when the retard/cut condition is triggered. This output can be routed into the **programmable outputs** system.

Workflow:
1. The ignition cut retard logic asserts its output signal.
2. That signal is mapped as a source in a programmable output.
3. The programmable output fires the solenoid when it detects the ignition cut retard signal.
4. Because the ignition cut retard sets the output slightly ahead of the actual cut, the solenoid gets its headstart before fuel/ignition is interrupted.

### Configuration Notes

- In TunerStudio/epicTuner, find the **Programmable Outputs** configuration section.
- Set one of the programmable output triggers to use the ignition cut retard output as its source condition.
- Adjust the timing offset or use the retard signal's lead time to ensure the solenoid is fully actuated before the ignition cut fires.
