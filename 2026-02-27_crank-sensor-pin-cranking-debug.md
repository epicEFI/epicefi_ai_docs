# Crank Sensor Input Configuration and Cranking Troubleshooting (epicEFI)

*Source: epicEFI Discord, 2026-02-27 | Channel: 1356732771325968630*
*Contributors: @Bloopdog, @ggurov*

## Summary

A new user (Bloopdog) working with an epicEFI adapter board asked where the "crank output" was in epicEFI. The clarification was that epicEFI does not output a crank signal — it reads an input from the crank sensor on whatever pin is configured in software. Later in the day, the same user reported that engine cranking produced no result despite having fuel pump, fans, CLT, IAT, flex fuel, injectors, and ignition all functional in test mode. The developer advised using the "Sync" section in the right-click menu as a software-based substitute for an oscilloscope.

## Details

### Crank Sensor Is an Input, Not an Output

- epicEFI/rusEFI does not produce a crank signal output. Terminology clarification:
  - "Crank output" (incorrect term) — does not exist.
  - **Crank sensor input** — the pin that reads the crankshaft position sensor signal.
- The crank input pin is **user-defined**: you choose any suitable pin in the software configuration and inform the firmware which pin to expect the signal on.
- The firmware then monitors that pin for the trigger pattern.

### Trigger Logger as a No-Scope Diagnostic Tool

- Without an oscilloscope, use the **trigger logger / Sync viewer** in the rusEFI/epicEFI software:
  - Right-click in the software → look for the **"Sync"** section.
  - This displays the trigger pattern as the ECU sees it, allowing verification that the crank signal is being received.
- In the trigger logger: a **red line** that jumps to the high end indicates a **trigger error** (sync loss).

### Bloopdog's Cranking Issue — Status

User had verified the following working in test mode before encountering the crank/no-start issue:
- Fuel pump
- Fans
- Coolant temperature sensor (CLT)
- Intake air temperature sensor (IAT)
- Flex fuel sensor
- Injectors
- Ignition (fires in test mode)

Despite all outputs working in test mode, cranking produced no combustion response. The user was working with an adapter board from dcwerx and awaiting a board layout/pinout from Speedyefi (Isaac) to track down potentially incorrect or missing pin assignments.

### Next Steps Recommended

1. Check the trigger logger (Sync viewer) during cranking to confirm the crank sensor signal is reaching the ECU.
2. Verify that the crank input pin configured in software matches the actual physical wiring on the adapter board.
3. Obtain the physical board pinout (from dcwerx adapter board documentation or Speedyefi) to confirm all signal paths.
4. If no crank signal appears in the trigger logger, troubleshoot the crank sensor wiring, pull-up/pull-down resistors, and sensor power supply.
