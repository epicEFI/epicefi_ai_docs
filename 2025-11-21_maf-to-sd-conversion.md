# Converting Stock MAF Tables to Speed Density (VE Table)

*Source: rusEFI Discord, 2025-11-21 | Channel: general/support*
*Contributors: dipsy, Joesphan*

## Summary

There is no single dedicated application to directly convert stock MAF (Mass Airflow) tables into a Speed Density VE table. The conversion requires back-calculating from the MAF airflow values to derive volumetric efficiency at corresponding RPM and load points. An AI assistant (e.g., ChatGPT) can help derive the back-calculation equation from the MAF transfer function.

## Details

**Problem:** A user wanted to take known stock MAF calibration tables and use them as a starting point for a Speed Density (VE-based) tune in rusEFI.

**Approach suggested by Joesphan:**

1. The MAF sensor provides a mass airflow reading (g/s or similar). The VE table in Speed Density mode is what rusEFI uses to calculate expected airflow from MAP, RPM, and engine displacement.
2. The conversion is a **back-calculation**: given known MAF values at specific operating points, use the SD airflow equation to solve for VE:

   ```
   VE = (MAF_measured * R_air * T_intake) / (MAP * engine_displacement * RPM/2)
   ```

   (with appropriate unit conversions)

3. Since the exact form of the equation can be non-trivial to set up, using an AI tool to generate or verify the equation from the MAF transfer function is a practical shortcut.

**Limitations:**

- Stock MAF tables only represent the operating points the OEM calibrated for. Extrapolation outside those points will not be reliable.
- This approach gives a reasonable starting VE table approximation but will still require closed-loop refinement (wideband O2 autotune) on the actual engine.
- No community-provided tool specifically automates this MAF-to-VE conversion for rusEFI at the time of this discussion.
