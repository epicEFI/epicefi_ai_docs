# Injector Pulse Width Linearity: New vs. Old Injectors

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: Joesphan (@joesphan)*

## Summary

Explanation of the fundamental difference in pulse width (PW) control behavior between modern injectors and older injector designs, and its impact on fuel quantity control accuracy.

## Details

**Modern (new) injectors:**
- Exhibit a **linear relationship** between pulse width and fuel delivered.
- Increasing PW by X% increases fuel quantity by approximately X%.
- This linearity makes fuel table calibration and closed-loop corrections straightforward.

**Older injectors:**
- Have a **flat spot** in the PW-to-fuel delivery curve at short pulse widths.
- In the flat spot region, changes in PW produce **no change in fuel delivery**.
- The root cause is **injector inertia**: the pintle/needle mass means the injector does not open fully (or at all) until a minimum PW threshold is exceeded.
- Below this threshold, the injector physically cannot respond to PW changes.

**Calibration implications for rusEFI/epicEFI:**
- When using older injectors, the injector short-pulse adder (also called the "small pulse width adder" or dead time correction) must be configured carefully to avoid operating in the flat spot region during idle or light cruise.
- Modern injectors typically have a characterized dead time (latency from PW command to actual opening) but are linear once open.
- If running old injectors, be aware that very short cranking pulses or idle trim corrections may fall into the non-linear region, causing fueling anomalies that cannot be corrected by simple trim adjustments.
