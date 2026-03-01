# BMW EKP Fuel Pump Control Module (CAN-Controlled H-Bridge)

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312 (theory/dev)*
*Contributors: @fast335xi, @joesphan, @ggurov*

## Summary

Discussion of the BMW EKP (Elektrische Kraftstoffpumpe) fuel pump control module, which uses an H-bridge configuration and communicates over CAN bus. The thread covers sniffing the OBD-II CAN bus to retrieve fuel pump data, the known failure modes of the OEM module, and a third-party upgraded replacement. Fuel temperature and fuel line sizing were also discussed in relation to high-performance applications.

## Details

### BMW EKP Module Overview

BMW vehicles (N54 and others) use a dedicated fuel pump control module called the EKP (Elektrische Kraftstoffpumpe = Electric Fuel Pump). Key characteristics:

- **Control method**: H-bridge circuit for variable speed pump control
- **Communication**: CAN bus (not a simple PWM or relay-switched output)
- The module receives torque/load signals over CAN and adjusts pump speed accordingly
- This is distinct from a simple PWM-controlled fuel pump — it requires CAN communication to command properly

### CAN Bus Sniffing for EKP Control

@fast335xi had already sniffed the OBD-II CAN bus for the EKP module:
- Plan was to "request and state on the CAN" — meaning request fuel pump data from the ECU and then re-broadcast it on the CAN bus
- This approach allows an aftermarket ECU (such as epicEFI) to control the EKP module by sending the appropriate CAN messages

### Known Failure Mode: Connector Melting

The OEM EKP module has a known weakness:
- The H-bridge uses wiring that is undersized for the current it handles
- Connectors are known to melt under sustained high-current operation
- The problem is compounded by the size of the wire used

### Third-Party Upgraded EKP Module

A third-party vendor (Arcterminator) sells an upgraded replacement:
- Upgraded H-bridge chip with higher current capacity
- Includes a small cooling fan for the module
- Upgraded wiring harness is also available
- Product link: https://arcterminator.com/products/upgraded-ekpm3

### Fuel Pump Control for DI (Direct Injection) Systems

For direct injection applications without the EKP module:
- @joesphan noted that DI requires PWM fuel pump control (for the low-pressure pump feeding the HPFP)
- An alternative for non-DI setups: run a return-style fuel system at full pressure and skip variable speed control
- rusEFI has a fuel pump PWM table for advanced fuel pump control
- A fuel pressure to deadtime table is also supported (relevant for dead-head/returnless systems that run like DI)

### Fuel Temperature Considerations

- The fuel rail can get notably warm, especially in return-style or high-demand setups
- @joesphan confirmed fuel rails get "uncomfortable to touch" after extended use
- rusEFI supports fuel temperature sensing through the flex fuel sensor input (which includes a temperature signal)
- @ggurov suggested using a PWM-controlled radiator fan (e.g., E46 fan PWM controller) to cool the fuel system if needed

### Fuel Line Sizing Recommendations

@fast335xi's recommendations based on experience:
- **6AN**: sufficient for up to ~1000 HP in some setups, but not recommended for ethanol at high power
- **8AN**: recommended for ~1000 HP on ethanol
- **10AN single**: used for setups without DI (single large line)
- Dual 6AN is an alternative for high-flow applications
- Running ethanol increases the volume of fuel needed vs. gasoline, affecting line sizing requirements
- An Aeromotive regulator returning fuel directly on top of the pumps helps with staged setups
