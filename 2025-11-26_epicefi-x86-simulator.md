# epicEFI x86 Software Simulator for Development and Tuning

*Source: rusEFI Discord, 2025-11-26 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), MarkCraftPerformance (@carolinacumberland)*

## Summary

epicEFI includes a built-in x86 software simulator in every firmware build. The simulator runs the exact same logic as the ARM/STM32 hardware target, exposes a TCP socket for TunerStudio connections, and includes CAN bus support. It eliminates the need for physical ECU hardware during development and plugin testing.

## Details

**What it is:**
Every epicEFI build produces an x86 binary alongside the ARM firmware. The x86 build strips out all STM32-specific hardware code and replaces it with software simulations of those peripherals, running natively on a Linux or Windows x86 machine.

**Connectivity:**
- Listens on a TCP port (same protocol as physical ECU serial/USB)
- TunerStudio connects to it identically to a real ECU
- CAN bus is also simulated, allowing CAN-dependent features to be tested

**Use cases:**
- Validating a new firmware build without flashing hardware
- Tuning and testing TunerStudio .ini / plugin changes without a physical ECU
- CI/regression testing of ECU logic changes

**epicTuner plugin testing via ECU simulator + OBD2 reader:**
MarkCraftPerformance described an additional testing approach: using a hardware ECU simulator/OBD2 reader device alongside the epicTuner plugin to validate behavior from both the TunerStudio side and the simulated ECU side simultaneously. This allows OBD2 protocol paths to be exercised as well, since his tooling has OBD2 protocols built in.

**WSL / Linux checkout note:**
When building epicEFI on Windows Subsystem for Linux (WSL), the repository must be checked out into the WSL home directory (not a Windows-mounted path like `/mnt/c/...`). Checking out into the WSL home directory avoids filesystem permission issues and ensures the build toolchain functions correctly.
