# Nitrous Staging Bug: Erratic Injector Outputs at 100% Staging

*Source: epicEFI Discord, 2026-02-21 | Channel: 1401895238481744052*
*Contributors: @tmbryhn, @ggurov, @Neos6443*

## Summary

A bug was identified in epicEFI's nitrous staging implementation where transitioning from 99% to 100% staging causes all eight defined injector outputs to behave erratically, even though the commanded pulse width (PW) in software reads correctly. Additionally, secondary injectors require a minimum of approximately 10% staging to begin activating. A deadtime calculation spike was also suspected as contributing to the erratic behavior.

## Details

### Confirmed Working Configuration (Tier 1 Checklist)

Prior to discovering the bug, the following nitrous configuration was confirmed functional:
- Nitrous uses TPS actual in ETB (Electronic Throttle Body) mode
- 100% staging allowed (configuration flag enabled)
- Batch fuel mode when cam signal is not present

### Bug: Injector Outputs at 100% Staging

**Symptom**: When staging steps from 99% to 100%, all 8 defined injector outputs begin acting erratically.

- At 99% staging: behavior is correct. Primary injectors receive only deadtime-equivalent pulses (near-zero fueling), secondary injectors fire normally.
- At 100% staging: all 8 injector outputs become erratic despite software reporting correct commanded PW (0 ms on primary, expected value on secondary).
- The commanded PW shown in the software gauge/log reads as expected — the problem is in the hardware output layer, not the fueling calculation.

**Suspected cause**: @ggurov suspected something is spiking the deadtime calculations, which would corrupt injector timing at the 100% boundary.

### Secondary Injector Minimum Staging Threshold

Secondary injectors require a staging percentage above approximately **10%** to initially activate. Below 10%, secondary injectors remain at 0 ms PW (inactive) even when commanded.

### Workaround

Define limits in the `.ini` configuration file:
- **Maximum staging: 99%** (avoid the 100% erratic output behavior)
- **Minimum staging: 10%** (ensure secondary injectors activate reliably)

This keeps the system within the known-working operational envelope until the underlying bug is resolved.

### Upstream Reporting Note

The issue was considered worth raising with the upstream rusEFI project, as they may encounter the same behavior when implementing 100% staging. However, care should be taken regarding how the discovery context (epicEFI) is disclosed when filing upstream reports.
