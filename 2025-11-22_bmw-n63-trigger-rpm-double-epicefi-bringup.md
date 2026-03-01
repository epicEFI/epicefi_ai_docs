# BMW N63 Trigger Configuration and RPM Doubling on epicEFI
*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: @Tera (teraflopping), @ggurov (ggurov), @MykkH (mykkh), @Joesphan (joesphan)*

## Summary

Documents the bring-up of a BMW N63 V8 engine on epicEFI (Proteus F4 board), covering a specific RPM doubling bug in TunerStudio when using the epicEFI firmware vs FOME, the root cause investigation (related to cam trigger configuration differences and an attempt to disable GDI), the N63 trigger pattern PR and the decision to use the universal trigger for faster iteration, plus the role of the GDI-8 external hardware module.

## Details

### Problem: RPM Reads Double in TunerStudio on epicEFI Firmware

When running the N63 on epicEFI firmware, TunerStudio reported RPM values at 2x the actual engine speed. The actual tacho signal sent over CAN by the factory DME was correct and unchanged — the dashboard read correctly. This was a TunerStudio display issue specific to the epicEFI firmware build, not a problem with the physical tach output (which on this setup is handled by the stock DME over CAN, not the Proteus).

Key clarification from Tera:
> "TS was reading double of what was *actually* happening. epic reads 2x of fome and the dashboard"

The FOME firmware build on the same Proteus F4 board did not show this problem — FOME recorded normal RPM values.

### Root Cause Investigation

MykkH identified a configuration difference between the FOME tune and the epicEFI tune:
- The FOME tune had cam trigger type set with **3 lobes** in the HPFP cam config
- The epicEFI tune had cam trigger lobes set to **0**

Tera clarified that the lobes were intentionally set to 0 in the epicEFI tune to disable cam inputs for isolated testing, specifically because GDI was stuck enabled and the cam inputs were interfering with sync. The crank trigger pattern itself is a standard **60-2** wheel, which was confirmed working and is present in the epicEFI dropdown.

MykkH also noted that the epicEFI tune was missing cam trigger input pin assignments entirely. This was intentional for the test session (to eliminate cam as a variable), but confirmed the tune needed a full table audit.

### GDI-8 Hardware

ggurov clarified that the N63 setup requires an additional external hardware module called the **GDI-8**:
> "there's an extra piece of hardware, GDI-8 — which is a passthrough for GDI injectors and a driver for the HPFP fuel pump capacity valve"

This module handles the high-voltage GDI injector drive (Gasoline Direct Injection requires a boost converter to generate ~65–90V pulses) and the HPFP (High Pressure Fuel Pump) solenoid control. Without this module, GDI injection cannot be controlled by the ECU.

The firmware was stuck requiring GDI mode to be active, which prevented disabling GDI inputs. Tera submitted PR #409 to the epicEFI firmware repo with an updated N63 trigger configuration to address this.

### Stock DME Sensor Sharing

The stock BMW DME continues to run in parallel during this bring-up phase. It still reads and processes:
- Cam position signals
- Crank position signals
- Additional sensors required for normal vehicle operation

The stock DME sends the tach signal to the instrument cluster via CAN bus, so any tach output from the Proteus is not connected to the dash in this configuration.

### Tune Verification Checklist for epicEFI

When migrating from FOME to epicEFI firmware or loading a new tune in epicEFI, MykkH recommended:
1. Load the epicEFI firmware onto the ECU
2. Open the project in the epicEFI-specific TunerStudio INI
3. Go through **every table** and verify there is no null, missing, or inaccurate data
4. Verify all input pin assignments — in particular, **cam trigger inputs** were missing from an earlier shared epicEFI tune

### Trigger Pattern Resolution

After the initial firmware-level investigation, the RPM doubling issue was resolved by switching to the **universal trigger** implementation (see separate doc on universal trigger). The tacho signal was confirmed normal after this change.

ggurov also noted that the epicEFI release INI files are available at:
`https://content.epicefi.com/firmware/ini/master/2025/11/`
