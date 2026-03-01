# MAX9924/MAX9926 Adaptive Threshold Failure with Low Tooth Count VR Sensors

*Source: rusEFI Discord, 2026-02-22 | Channel: 1356732771325968630*
*Contributors: @Ognjen Galic (@ogalic), @Joesphan (@joesphan)*

## Summary

Ognjen Galic identified a root cause for why the MAX9924 and MAX9926 VR sensor conditioner ICs fail on crankshaft trigger wheels with a low tooth count. The chips use an adaptive threshold that has an 85 ms watchdog: if no output pulses are generated within 85 ms (which happens easily at cranking speeds with few teeth), the chip resets the adaptive threshold to zero. This allows line noise to be interpreted as valid trigger pulses during the reset window, producing false triggers. The recommended workaround is to switch the chip to "B mode" (fixed threshold, disabling the adaptive threshold) and configure a reasonable fixed threshold voltage.

## Details

### Root Cause: 85 ms Adaptive Threshold Watchdog

The MAX9924/MAX9926 ICs contain a watchdog timer that resets the adaptive threshold to 0 V if no output pulse is generated within 85 ms. On low tooth count trigger wheels at cranking RPM, the inter-tooth interval can exceed 85 ms, causing the threshold to reset repeatedly. With a reset threshold of 0 V, electrical line noise on the VR sensor wiring is sufficient to generate false trigger pulses.

- Chip: MAX9924 / MAX9926
- Watchdog period: 85 ms
- Effect: adaptive threshold resets to 0 V on watchdog expiry
- Result: line noise triggers false output pulses during cranking with low tooth count wheels

### The watchdog cannot be disabled independently

The adaptive threshold watchdog is not independently disableable in the MAX9924/MAX9926. Disabling adaptive mode (Mode B) is the only way to prevent the watchdog reset behavior.

### Fix: Switch to Mode B (Fixed Threshold)

Switching the chip to B mode disables adaptive thresholding entirely. A reasonable fixed threshold voltage must then be set manually.

- Switch IC to B mode (disables adaptive threshold)
- Set a fixed threshold voltage appropriate for the VR sensor signal amplitude
- The threshold is configurable from 0 V (min) to 1.4 V (max)
- An RPM-vs-voltage curve set by firmware would be the ideal future enhancement (threshold could be adjusted dynamically based on engine speed to handle both cranking and running signal amplitudes)

### Behavior and Applicability

- The MAX9924/MAX9926 is well-suited for high tooth count wheels (e.g., ring gears) where inter-tooth intervals remain well below 85 ms even at low RPM
- It is specifically problematic for low tooth count wheels at cranking, where inter-tooth gaps can exceed the watchdog period
- Other ECU vendors (Microsquirt, MS3 Pro) have moved away from discrete VR conditioner circuits and are now using the MAX9926 in full automatic mode, which carries the same risk on low tooth count applications

### Note on Non-Adaptive Mode Tradeoffs

Running in non-adaptive (fixed threshold) mode can become suboptimal at higher RPM because the VR sensor signal amplitude changes significantly with speed. A fixed threshold set for cranking may be inappropriate at high RPM. Firmware-controlled dynamic threshold adjustment (RPM-vs-voltage table) was suggested as a better long-term solution.
