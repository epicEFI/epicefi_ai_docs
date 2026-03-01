# VR Conditioner Behaviour with Low Tooth-Count Triggers: MAX9924/9926 Analysis

*Source: rusEFI Discord, 2026-02-22 | Channel: 1356732771325968630*
*Contributors: @Ognjen Galic, @Joesphan, @Robb235, @ZeeKay*

## Summary

A detailed technical discussion about the failure mode of Maxim MAX9924/9926 VR conditioner ICs when used with low tooth-count trigger wheels (single-tooth cam triggers, ring gears at cranking speed, etc.). Simulation work in Multisim confirmed the root cause: an 85 ms internal watchdog resets the adaptive threshold to zero when no output pulses are generated, causing false triggering on line noise. The fix is to disable the adaptive threshold (Mode B) and set a fixed comparator threshold voltage.

## Details

### The Problem: Adaptive Threshold and the 85 ms Watchdog

The MAX9924/9926 has an internal 85 ms watchdog timer. If no output pulse is generated within 85 ms (i.e., the input signal period is longer than 85 ms), the chip resets its adaptive threshold to 0 V.

At 0 V threshold, the chip triggers on line/electrical noise, producing spurious output pulses. With a low tooth-count trigger at cranking speeds:
- Tooth period easily exceeds 85 ms (e.g., a single cam tooth at 200 RPM = 300 ms per revolution)
- Threshold resets to zero → chip fires on noise → ECU sees phantom teeth → flooding from spurious injector pulses

Simulation confirmed this with a 1 Hz sine wave (representing ~60 RPM or a 1-tooth-per-rev cam trigger):
- **Mode A/A2 (adaptive)**: garbage output — false triggers on noise
- **Mode B (fixed comparator, adaptive disabled)**: clean output

### Affected Products

- Speeduino "banana" VR conditioner board — uses the NXP MC79067 (original), which has similar characteristics; does not like low tooth counts
- MicroSquirt and MS3Pro — ditched their custom discrete VR conditioner and switched to the MAX9926 in full automatic mode, which suffers from this exact problem, and with notably poor decoupling

### Workaround / Fix

1. **Put the MAX9924/9926 in Mode B** (disable adaptive threshold, use fixed comparator mode)
2. Set a **fixed threshold voltage** — the chip supports 0 V minimum to 1.4 V maximum
3. Optionally, ggurov noted the threshold could be set dynamically by firmware using an RPM vs. voltage curve

Note: the adaptive threshold watchdog **cannot be disabled separately** — the only way to avoid it is to use Mode B entirely.

### Hardware Notes

- Robb235 built a custom carrier board using the **LM1815** (National Semiconductor/TI, classic DIP package, now also available in SOIC) for his uaEFI to handle Toyota shared-ground VR sensors
  - Board size: 25 mm × 25 mm
  - The LM1815 is "dinosaur tech" but reliable; VEMS has used it historically, as has Link G2
  - It handles Toyota shared-sensor-ground wiring schemes that give the MAX9926 trouble

### Toyota Shared Ground Issue

Toyota (and some Honda) VR sensors share a common negative/ground wire between multiple sensors (crank + cam, or crank + cam + other). This creates ground reference issues for VR conditioners that expect a dedicated differential input. Robb235's LM1815-based board was specifically designed to cope with this.

### Banana Board (Speeduino VR Conditioner)

The "banana" refers to the Speeduino project's yellow VR conditioner board (NXP MC79067-based). It is good for high tooth-count applications (ring gears, 36-tooth wheels at normal RPM) but fails with very low tooth counts for the same reason — inadequate handling of long inter-tooth intervals. A new version was being planned with status LEDs and a compact single-channel form factor.
