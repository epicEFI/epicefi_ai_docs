# B48/B58 Brushless Fuel Pump Replacement with Walbro 525

*Source: epicEFI Discord, 2026-02-21 | Channel: 1356732771325968630*
*Contributors: @fast335xi*

## Summary

The B48 and B58 engines use a brushless in-tank fuel pump controlled by an Electronic Fuel Pump regulator (EKP). When converting to a standalone ECU setup, the stock brushless pump and EKP are typically deleted and replaced with a conventional Walbro 525 pump and an aftermarket FPR. An open question remains about whether the EKP communicates via FlexRay or CAN.

## Details

### Stock Configuration

- B48/B58 engines use a **brushless in-tank fuel pump** controlled by an **EKP (Electronic Fuel Pump regulator/controller)**.
- The EKP controls pump speed electronically rather than using a simple relay.

### Replacement Procedure (fast335xi's approach)

1. **Delete the EKP** — remove the electronic fuel pump controller from the system.
2. **Install a Walbro 525** — drop-in conventional brushed fuel pump replacement.
3. **Block the stock FPR location** — thread a bolt into the stock fuel pressure regulator port to seal it.
4. **Install an aftermarket FPR** — mount a conventional mechanical fuel pressure regulator.

### Open Question

- It is unclear whether the stock EKP communicates via **FlexRay or CAN**. This matters if any other module on the car expects communication from the EKP (e.g., for fault suppression or pump speed feedback). If the car throws faults due to a missing EKP, additional fault suppression (termination resistors, emulator) may be needed.

### Related Hardware Note

- The **N55 oil pressure sensor connector is the same as the B48/B58 fuel pressure sensor connector** — useful for sourcing compatible connectors during a fuel system build.
