# VR Sensor Sensitivity: Tooth Shape, Gear Iron Content, and Sensor Gap

*Source: rusEFI Discord, 2025-11-18 | Channel: 1435631740969291796*
*Contributors: @Chemex-joshseek*

## Summary

A brief but important note on VR (variable reluctance) sensor behavior: sensor accuracy and reliability is affected not just by gap distance but also by the shape of the trigger wheel teeth and the iron (ferrous) content of the gear material. Reducing sensor gap does not always improve performance and can in some cases degrade it.

## Details

### Key Points

Chemex-joshseek (joshseek_30236_07984) noted:

> "Some sensors can be sensitive to that shape of teeth, and iron content of that gear. Also sensor gap closer doesn't always mean better."

This observation was made in the context of a broader discussion about VR conditioner and trigger sync issues with a Honda-style distributor wheel.

### Practical Implications

- **Tooth geometry matters**: VR sensors generate a voltage pulse from the rate of change of magnetic flux as a tooth passes. Teeth that are too narrow, too tall, too shallow, or have irregular profiles can produce asymmetric, weak, or noisy signals — even if the sensor gap is within spec.
- **Iron content of the trigger wheel**: VR sensors rely on the ferromagnetic properties of the trigger wheel material to generate a signal. Wheels made from lower-permeability alloys (e.g., some cast or sintered metals with low iron content) produce weaker signals than high-quality steel wheels. Aftermarket or custom trigger wheels should be made from mild steel or other high-permeability ferromagnetic material.
- **Closer gap is not always better**: Reducing the gap between sensor and wheel increases peak signal amplitude, but can also cause:
  - Physical contact risk if the wheel is not perfectly concentric
  - Signal clipping or saturation in the VR conditioner at high RPM
  - Increased sensitivity to wheel runout and vibration
  - In some conditioner designs, the adaptive threshold may not track correctly when peak amplitudes are too high

### Recommendation

When diagnosing VR trigger issues, do not assume that closing the gap will fix a weak or noisy signal. Instead:
1. Verify the trigger wheel is made from appropriate ferromagnetic steel
2. Inspect tooth profile for damage, uneven wear, or manufacturing irregularities
3. Set the gap to the manufacturer's specified range (typically 0.5–1.5 mm for most automotive trigger wheels)
4. If signal quality is still poor, consider a VR conditioner with better adaptive threshold behavior (e.g., MAX9924, MAX9926, or purpose-built modules like the WTM Tronics JDM VR conditioner)
