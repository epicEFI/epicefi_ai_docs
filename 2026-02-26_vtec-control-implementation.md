# VTEC Control Implementation: Settings, Oil Pressure Interlock, and Override Input

*Source: rusEFI Discord, 2026-02-26 | Channel: 1401895238481744052 (general-tech)*
*Contributors: @BossTheTuga, @ggurov, @sheckta#1593*

## Summary

This thread documents the development and discussion of VTEC control functionality in rusEFI/epicEFI, covering: the recommended control settings (ON threshold + hysteresis), limitations of the GP PWM workaround, the addition of an oil pressure interlock (digital or analog), an override input with signal inversion support, and a known issue where VTEC configured to deactivate at high RPM breaks the hysteresis mechanism.

## Details

### Recommended VTEC Control Settings

Two settings are considered sufficient for most use cases:

1. **VTEC ON threshold + hysteresis** — activate VTEC at a given RPM and hold it active until RPM drops below (threshold - hysteresis)
2. **Simple ON/OFF without hysteresis** — straightforward toggle at a single RPM setpoint

Having three distinct activation settings was questioned as unnecessary. The hysteresis approach (option 1) covers the majority of VTEC-equipped Honda/Acura applications.

### GP PWM Workaround (Avoid)

Prior to dedicated VTEC output support, some users implemented VTEC solenoid control via a General Purpose PWM (GP PWM) output. This approach:
- Does **not** cause hardware damage
- Is described as "wonky" — unreliable or unstable behavior reported
- Should be replaced with the dedicated VTEC output once available

### Known Issue: VTEC Deactivation at High RPM Breaks Hysteresis

If the VTEC configuration includes a **deactivation RPM** (e.g., turning VTEC OFF above a high RPM ceiling), the hysteresis mechanism malfunctions. @BossTheTuga notes that deactivating VTEC at high RPM is also logically counterproductive for most engines. This is a known firmware bug to be addressed.

### Oil Pressure Interlock for VTEC

VTEC systems require sufficient oil pressure to engage the variable cam mechanism. To prevent engagement without adequate oil pressure:

- **Digital input (recommended for stock engines)**: Most OEM VTEC engines use a simple on/off oil pressure switch. A digital input monitoring the switch state should gate VTEC activation — if the switch is not triggered, VTEC will not activate regardless of RPM.
- **Analog input (optional)**: For users with a continuous oil pressure sensor, an analog input can be used to gate VTEC based on a minimum pressure threshold.

@ggurov framed this as a "minimum oil pressure option" in the firmware.

Note: Many users run VTEC engines without the oil pressure switch connected at all. The interlock is optional but recommended for safety.

### Override Input (Added)

@ggurov added an **"override" input** for VTEC, allowing an external signal (e.g., a switch, relay output, or another ECU output) to force VTEC state independently of the RPM/load table.

Per @BossTheTuga's recommendation, the override signal should support **signal inversion** — so users can configure the override as either active-high or active-low, depending on their wiring.

### Programmable Outputs for Reverse Lock Solenoid (K20)

During the same development session, @BossTheTuga tested a reverse lock solenoid on a K20 engine using programmable outputs:

- **Programmable ports menu**: Did not work — output appeared to always evaluate to false. @Joshan Lu suggested an incorrect configuration may have caused the always-false condition.
- **Logic outputs**: Worked correctly for the same solenoid and configuration
- **Workaround**: Configure the reverse lock solenoid via Logic Outputs rather than Programmable Ports; use RPM as the activation condition for easy bench testing

### Enabling Programmable Outputs (STATIC_BOARD_ID)

Programmable outputs require the correct board ID to be set in the firmware build. The following compiler define enables programmable outputs:

```
DDEFS += -DSTATIC_BOARD_ID=444
```

Without this define, the programmable outputs feature is not active for the target board. After adding this define and rebuilding, programmable outputs functioned correctly.

### User Curves with Logic Output

Logic outputs (rather than user curves directly) are the supported way to implement curve-based switching logic in the current firmware. @ggurov asked about user curve support; @BossTheTuga confirmed that logic outputs achieve equivalent functionality and demonstrated working configuration.

## Notes

- VTEC oil pressure switch wiring: the stock Honda/Acura switch is a normally-open contact that closes when oil pressure is sufficient. Wire to a digital input configured as active-low (or use the invert option)
- The `DDEFS += -DSTATIC_BOARD_ID=444` fix for programmable outputs may be board-specific; confirm the correct ID for your target hardware
- GP PWM VTEC control should be considered deprecated once the dedicated VTEC output is stable
