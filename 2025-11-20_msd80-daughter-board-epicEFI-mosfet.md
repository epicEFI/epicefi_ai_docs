# MSD80 Daughter Board Diagnosis and epicEFI MOSFET Triggering

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: fast335xi (@fast335xi)*

## Summary

Approach for diagnosing the MSD80 ignition module's daughter board by desoldering it and testing whether epicEFI can trigger it via the MOSFETs on the main board.

## Details

**Context:**
The BMW MSD80 is a combined DME (engine control unit) and ignition driver module used on N54/N55 engines. It contains a main board and a daughter board. When adapting this hardware or diagnosing faults, isolating the daughter board from the main board is a useful diagnostic step.

**Diagnosis procedure:**
1. **Desolder the daughter board** from the MSD80 main board to isolate it and examine its behavior independently.
2. The main board contains **MOSFETs** that are used to trigger the daughter board — these are the interface points between the ECU logic and the high-current ignition outputs.
3. **Test with epicEFI:** Connect the isolated assembly to an epicEFI controller and test whether epicEFI's outputs can trigger the daughter board through the same MOSFET interface. If it responds correctly, the daughter board is functional and the issue lies in the main board logic or power supply.

**Key takeaway:**
This is a hardware-level diagnostic method — by bypassing the MSD80's own MCU and driving the MOSFETs directly from epicEFI, you can determine whether the daughter board (ignition coil drivers) is serviceable. If epicEFI can successfully trigger it, the daughter board can potentially be reused in a standalone epicEFI installation.
