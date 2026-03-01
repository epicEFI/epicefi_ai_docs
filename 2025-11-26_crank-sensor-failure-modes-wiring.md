# Crank Position Sensor Failure Modes and Wiring on Race Bikes

*Source: rusEFI Discord, 2025-11-26 | Channel: 1411925674498719775*
*Contributors: Joesphan (@joesphan), Lysdex (@lysdex), benassi125 (@benassi125)*

## Summary

Discussion of recurring crankshaft position sensor (CKP) failures on race motorcycles despite high-quality wiring and shielding. Heat is identified as the likely culprit. A related point on coil type and heat resistance is included.

## Details

**Failure pattern:**
CKP sensors can fail repeatedly even when wiring is done correctly with premium aerospace-grade cabling. A race bike application reported recurring failures despite the following wiring spec:
- **Cable type:** M27500 shielded cable
- **Outer sleeve:** DR25 heat-shrink tubing (polyolefin dual-wall)
- **Abrasion protection:** HFT5000 sleeving over the DR25

Despite this multi-layer protection on the wiring harness itself, the sensors continued to fail. The suspected root cause is heat at the sensor body/junction rather than wiring degradation — the wiring survived but the sensor element did not.

**Heat as the primary cause:**
On race motorcycles, the CKP sensor is typically positioned close to the engine cases and is exposed to radiated heat from the engine and exhaust. The internal silicon-based junction in a typical Hall-effect or VR sensor is more susceptible to thermal damage than the external wiring. Potted coil-type sensors (where the internal windings are encapsulated in epoxy/resin) are more thermally robust than non-potted equivalents because the potting compound dissipates and insulates heat more effectively.

**Coil type comparison (heat resistance):**
- **Non-potted / silicon-junction coils:** more prone to heat failure; air gaps in the housing allow thermal cycling damage
- **Potted coils:** better heat resistance; epoxy encapsulation protects the junction from thermal cycling and vibration

**Recommendations for high-heat applications:**
- Verify the sensor body temperature rating, not just the cable rating
- Prefer OEM sensors known to survive the specific engine's heat soak (or aftermarket sensors with full epoxy potting)
- Consider heat shielding or re-routing at the sensor body mounting point, not just the cable run
- On race bikes, inspect and replace CKP sensors proactively rather than reactively
