# High-Pressure Fuel Pump (HPFP) Dual-Bank Independent Control

*Source: rusEFI Discord, 2025-11-27 | Channel: 1356732771325968630*
*Contributors: Tera (@teraflopping), ggurov (@ggurov), Joesphan (@joesphan)*

## Summary

rusEFI/epicEFI supports independent control of two high-pressure fuel pump channels (hpfp1 and hpfp2, referred to as Bank 1 and Bank 2). On 2025-11-27, Tera confirmed that independent HPFP bank control was working on their setup, with Bank 1 stable and Bank 2 initially lagging but ultimately reaching commanded pressure. If errors appear on firmware burn, a power reset of the ECU followed by a TunerStudio reboot clears them.

## Details

### Independent Bank Control

The firmware exposes two separate HPFP control channels. Each bank can be commanded to a different target pressure independently:

- **Bank 1 (hpfp1):** Reported stable and responding correctly
- **Bank 2 (hpfp2):** Was confirmed working previously; during testing showed a small lag when transitioning between pressure targets but did reach commanded values

Test sequence observed:
```
Commanded: 20 → 30 → 10 → 40 (MPa or bar, per tune setup)
Bank 1: responded immediately
Bank 2: slight delay transitioning from 30 → 10, but reached target
```

Both channels were confirmed visually via oscilloscope/logging (yellow and green traces per bank).

### Startup Behavior Before HPFP Control Was Working

Prior to getting independent HPFP control functional, the engine exhibited "really shitty starting characteristics" — it was reluctant to start. Once both banks were under closed-loop control the starting behavior normalized.

### Error Recovery After Firmware Burn

When errors or unexpected states appear immediately after flashing new firmware (reported as errors that "appear upon burn"):

1. **Power reset the ECU controller** (full power cycle, not just a reset button)
2. **Reboot TunerStudio (TS)**

This clears the spurious post-flash errors and restores normal communication. Suggested by MykkH, confirmed effective by Tera.

### hpfp2 Status

ggurov asked directly whether hpfp2 was functional (`hpfp2 is working?`). Tera confirmed: *"it was, yes"* and planned further testing the following day. This indicates the hpfp2 channel in the epicEFI firmware was at minimum operational in bench/garage testing as of late November 2025.

### Notes

- Bank 2 angle configuration matters: Tera initially reported Bank 2 as unstable, then found it was a "wrong angle" setting — correcting the angle parameter resolved the issue.
- CAN bus sniffed data (transmission data) should be backed up before any configuration changes, as it can be accidentally deleted during testing sessions.
