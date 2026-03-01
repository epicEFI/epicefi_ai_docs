# ECU Power Management and Post-Key-Off Operations (Turbo Timer, Fan Control, Config Save)
*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @Mitch (mitch0s), @Joesphan (joesphan)*

## Summary

Describes the standard MOSFET-based power hold circuit used in ECUs to enable post-ignition-off tasks such as saving configuration, running a turbo timer, and keeping cooling fans active. Also covers the specific constraint on the RP2040 microcontroller where XIP (Execute In Place) prevents flash writes while firmware is running, requiring all data to remain in RAM until after key-off.

## Details

### Power Hold Circuit

The conventional approach for post-key-off ECU operation uses two power inputs:
- **Constant 12V** supply (always live)
- **IGN 12V** input (switched by ignition key)

How it works:
1. When the ignition key is turned on, the IGN 12V line switches a MOSFET that connects the constant 12V supply to power the MCU and other ECU components.
2. While the MCU is running, it also drives the MOSFET gate high itself, holding itself powered independently of the IGN line.
3. When the key is turned off, the IGN line drops, but the MCU detects this and continues to hold the MOSFET gate high.
4. The MCU then performs all post-key-off tasks:
   - Disable all outputs
   - Save/flash configuration to non-volatile storage
   - Run turbo timer (keep boost pressure safe)
   - Keep cooling fans running for a configured number of seconds
5. After completing these tasks, the MCU releases the MOSFET gate, cutting power to itself.

### Use Cases for Post-Key-Off Operation
- Writing configuration while engine is off
- Turbo timer (maintaining oil circulation after boost)
- Keeping fans on for a configurable number of seconds after engine off
- Running diagnostics

### RP2040 XIP Flash Constraint

The RP2040 microcontroller uses XIP (eXecute In Place), meaning it executes firmware directly from flash memory at runtime. This creates a constraint:

- Flash read/write operations halt the entire firmware execution on the RP2040
- All data intended for flash storage must remain in RAM during normal operation
- Flash writes can only safely occur after key-off, when it is safe to halt execution

This is why the power-hold circuit is especially important on RP2040-based ECU designs — it provides the window of time after key-off during which the RP2040 can safely write configuration and learned data to flash.

The Joesphan build uses a `vpp` (programming voltage / auxiliary power) mechanism for this purpose.
