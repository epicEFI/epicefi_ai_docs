# Fuel Pressure Regulator (FPR) Tuning and Pressure Settings

*Source: epicEFI Discord, 2026-02-26 | Channel: 1411925674498719775*
*Contributors: @YeOldePirate, @fast335xi, @Joesphan*

## Summary

This thread covers how to manually adjust an adjustable fuel pressure regulator (FPR) and what pressure values to target. The process involves running the fuel pump with the manifold vacuum reference disconnected so you can set the static (open-air) base pressure, then reconnecting the reference line for normal operation.

## Details

### FPR Adjustment Procedure

1. **Disconnect the manifold reference line** from the FPR vacuum port.
2. **Plug the disconnected hose** with a cap or your finger to prevent it from acting as a vacuum leak into the intake.
3. **Run the fuel pump** (engine running or using a fuel pump prime cycle — either works for static adjustment).
4. **Turn the FPR adjustment screw** (typically clockwise to increase pressure, counter-clockwise to decrease) until the fuel pressure gauge reads the desired open-air (static) base pressure.
5. Once satisfied with the static pressure, **reconnect the manifold reference line**. Pressure will drop at idle due to the vacuum signal pulling on the FPR diaphragm.

### Target Pressure Values

These values were shared as reference points for a typical returnless or return-style setup:

| Condition | Pressure |
|-----------|----------|
| Open air (static, no vacuum reference) | 3.0 bar |
| At idle (with manifold vacuum) | ~2.5 bar |
| Open air (higher-performance tuning) | 3.5 bar |

- The ~0.5 bar drop from static to idle is normal and expected when the FPR has a manifold reference; as vacuum increases at idle, the FPR lowers rail pressure proportionally.
- Some users report better top-end power at **3.5 bar open-air** static pressure, particularly on applications with larger injectors or higher-flowing setups. This shifts the entire pressure curve up by 0.5 bar.

### Notes

- Do not adjust the FPR with the manifold reference line still connected, as intake vacuum will vary and make it impossible to set a stable static pressure.
- Always use a quality fuel pressure gauge for adjustment — the software fuel pressure display (if using a MAP-referenced virtual sensor) will only be accurate after the reference line is reconnected.
- After setting pressure, verify idle and WOT fuel pressure to ensure the regulator is tracking correctly.
