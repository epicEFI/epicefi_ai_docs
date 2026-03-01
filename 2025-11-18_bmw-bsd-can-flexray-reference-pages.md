# BMW BSD, CAN, Flexray, and Related System Reference Page Numbers

*Source: rusEFI Discord, 2025-11-18 | Channel: 1379478511667908618*
*Contributors: @fast335xi*

## Summary

Reference page numbers from what appears to be a BMW factory technical documentation (likely ETK or WDS/ISTA wiring diagrams or repair manual) for key vehicle bus systems and components. Useful when working on BMW engine swaps or ECU integration projects involving BMW buses and peripheral systems.

## Details

### Reference Page Index

@fast335xi shared the following page references:

> "Page 1180 for BSD. Page 1282 & 1633 for CAN. Page 1456 waterpump control. Flexray page 1559. EKP fuel pump 1621. EGS 8hp CAN 1644. DSC on flexray 1652. CAS 1315. Crank 2786"

Organized as a table:

| Component / System | Page(s) |
|-------------------|---------|
| BSD (Bit Serial Data) | 1180 |
| CAN bus | 1282, 1633 |
| Water pump control | 1456 |
| FlexRay bus | 1559 |
| EKP (electric fuel pump) | 1621 |
| EGS 8HP (8-speed automatic gearbox CAN) | 1644 |
| DSC (Dynamic Stability Control) on FlexRay | 1652 |
| CAS (Car Access System / immobilizer) | 1315 |
| Crank sensor | 2786 |

### Context

These references are relevant for rusEFI users performing BMW engine swaps or integrating rusEFI with BMW peripheral systems (electric water pump, fuel pump control, CAN-connected transmission, etc.). The BMW E-series and F-series platforms use a mix of CAN, BSD, and FlexRay for different subsystems:

- **BSD** is used for older BMW infotainment/body control communication
- **CAN** is the primary powertrain and chassis bus
- **FlexRay** is used on newer/higher-end BMW platforms for safety-critical systems (DSC, active steering)
- **EKP** is BMW's terminology for the electronically controlled fuel pump module, which requires a CAN command to enable
- **EGS 8HP** refers to the ZF 8-speed automatic transmission (used widely in BMW, Audi, and other platforms), which communicates over CAN and requires specific messages for proper gear position feedback and torque management
- **CAS** is the BMW immobilizer/key system, often relevant when performing an engine swap to understand what signals need to be emulated or bypassed
