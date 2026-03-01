# STFT Behavior with "Stop STFT for Rot Idle" Setting

*Source: rusEFI Discord, 2026-02-21 | Channel: 1464230782905352261*
*Contributors: @MykkH*

## Summary

A specific STFT (Short-Term Fuel Trim) interaction was documented: when the "Stop STFT for Rot Idle" setting is set to False, STFTs become active (i.e., fuel trims are applied) when Rot Idle is disabled. This is a counterintuitive behavior worth noting when tuning fuel trims around idle conditions.

## Details

### Setting Behavior

The **"Stop STFT for Rot Idle"** option controls whether short-term fuel trims are suspended during rotational idle (rot idle) conditions.

Observed behavior:
- `Stop STFT for rot idle: False` — STFTs are **not** suspended during rot idle. When Rot Idle mode is subsequently **disabled**, STFTs become active/alive and begin adjusting fueling.

This means that setting this option to `False` allows STFT corrections to run even through idle transitions. If unexpected fuel trim activity is observed when transitioning into or out of rot idle, check this setting.

### Practical Notes

- If STFTs are unexpectedly active after disabling Rot Idle, verify this setting is not set to `False` unintentionally.
- For stable idle tuning sessions where trim learning should be paused during rot idle, set "Stop STFT for Rot Idle" to `True`.
- This behavior was observed during active tuning; no firmware version was specified.
