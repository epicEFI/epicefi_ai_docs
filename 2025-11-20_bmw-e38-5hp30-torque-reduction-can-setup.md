# BMW E38 5HP30 Torque Reduction via CAN Bus: Initial Setup and CAN ID Reference

*Source: rusEFI Discord, 2025-11-20 | Channel: 1439273357424988301*
*Contributors: @sansvirus. (sansvirus.), @ggurov (ggurov), @SeV (sev), @GGGI (.gunadi), @fast335xi (fast335xi)*

## Summary

Initial community investigation into implementing CAN-bus-based torque reduction requests for a BMW E38 with the ZF 5HP30 automatic transmission. ggurov confirmed that the CAN virtual input feature can be used for this purpose. SeV identified the relevant CAN ID (0x43F) for listening to torque reduction requests on this platform. The ZF 8HP was also being manually reverse-engineered in parallel.

## Details

### Application Context

**Vehicle:** BMW E38
**Transmission:** ZF 5HP30 (hydraulic automatic, electronically controlled via CAN — sansvirus. confirmed: "Its electronically controlled, she dialog via CAN with the ECU")

**Goal:** Request torque reduction from the ECU via the CAN bus during gear shifts, to reduce driveline shock when the automatic transmission upshifts. This is the standard method used by BMW OEM TCUs.

### CAN Virtual Input Confirmation

User sansvirus. asked whether the **CAN virtual input** feature in rusEFI/epicEFI could be used to trigger a torque reduction request received from the transmission over CAN.

ggurov confirmed: **yes, CAN virtual input can be used for this**.

sansvirus. confirmed they would test this setup and report back. ggurov requested they keep the community updated, noting it could become a preset option for this class of setup.

### CAN ID for Torque Reduction

SeV (an experienced community member) identified the CAN ID to monitor for torque reduction requests:

- **CAN ID: 0x43F** — torque reduction request on BMW E38 / 5HP30

This is the address the transmission broadcasts when requesting the engine reduce torque during an upshift.

### ZF Transmission Type Clarification

GGGI asked for clarification on the gearbox type:
- fast335xi noted: ZF 8HP is "hydraulic but controlled electronically" — standard automatic planetary gearbox with electronic solenoid control, not a mechatronic DSG/DCT type.
- The 5HP30 (sansvirus.'s gearbox) is the same category.

### ZF 8HP Manual CAN Definition Work

In parallel, fast335xi was manually reverse-engineering the **ZF 8HP** CAN protocol:
- All DTCs in the 8HP that reference CAN IDs were being catalogued.
- fast335xi noted: "Just spent all night manually defining the 8hp."
- An offer was made to analyze any available BIN files for transmission/engine combinations to accelerate the process.

**Notable ZF transmissions discussed as targets for CAN integration:**
- **ZF 5HP30** — BMW E38 (sansvirus.'s application)
- **ZF 6HP28** — described as "about it" for 6-speed ZF worth supporting
- **ZF 8HP** — being actively defined

### Hardware Note

A custom **nanoTCU / nano gateway** device was mentioned as the interface hardware being used by fast335xi for CAN testing alongside the epicEFI ECU. The device allows swapping between the epicEFI CAN and other CAN devices during development testing.

---

**Follow-up:** See `2025-11-22_torque-reduction-can-input-bug-ignition-table-workaround.md` for the subsequent bug fix and workaround for the CAN torque reduction input implementation.
