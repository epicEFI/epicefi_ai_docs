# Magnetic Pickup Signal Conditioning and Single-Pickup Timing Accuracy

*Source: rusEFI Discord, 2025-11-25 | Channel: 1401895238481744052*
*Contributors: mynameisdeleted (@mynameisdeleted)*

## Summary

A JK flip-flop circuit with appropriate resistors and protection diodes is a reliable method for converting a variable-reluctance (magnetic pickup / VR) sensor output into clean digital edges for ECU trigger inputs. When a single pickup is used on 1- or 2-cylinder engines, only the negative zero-crossing (negative edge) provides a timing-accurate reference aligned to TDC; the positive edge near BDC has up to ±20° of jitter.

## Details

**Pickup conditioning circuit — JK flip-flop approach:**
A variable-reluctance (magnetic/inductive) pickup produces a sinusoidal AC signal whose amplitude and frequency vary with engine speed. Converting this to reliable digital edges requires:

- A JK flip-flop (e.g., 74HC76 or equivalent) to detect zero-crossings and produce clean logic-level transitions
- Correct pull-up/pull-down resistors to bias the input and set switching thresholds
- Protection diodes to clamp voltage spikes from the inductive pickup coil

This circuit is described as turning "magnetic pickup to reliable digital edges." The advantage over a simple comparator is that the JK flip-flop's toggle behavior provides hysteresis-like noise immunity on marginal signals.

**Single-pickup timing accuracy on 1- and 2-cylinder engines:**
When only one magnetic pickup is used (no separate cam/sync sensor), timing accuracy depends on which zero-crossing edge is used:

| Edge | Location | Timing Accuracy |
|------|----------|-----------------|
| Negative zero-crossing | Top Dead Center (TDC) | Sharp and reliable |
| Positive zero-crossing | Near Bottom Dead Center (BDC) | Variable, ±20 degrees |

> "Only sharp 0-crossings have reliable time, so on 1-cyl or 2-cyl with single pickup the negative edge is tdc, and positive edge is variable but near bdc +-20 degrees" — mynameisdeleted

The negative edge is sharp because TDC is a point of rapid flux change as the reluctor tooth passes the pickup at the closest point. The positive edge near BDC is less consistent because the magnetic field change is more gradual and varies with speed and tooth geometry.

**Series/reverse-polarity pickups — both edges usable:**
> "On series reverse polarity pickups, both edges are good" — mynameisdeleted

When two pickups are wired in series with reversed polarity, the signal waveform is symmetrical and both zero-crossings become equally sharp, allowing both edges to be used as reliable trigger events. This doubles the effective resolution for single-cylinder applications.

**Practical recommendation:**
- For single-pickup 1- or 2-cylinder setups: configure the ECU trigger to use only the negative edge (TDC reference); discard or ignore the positive edge.
- If higher resolution is needed: use a series reverse-polarity pickup pair to make both edges valid.
- Always condition VR sensor output through a dedicated circuit (JK flip-flop or dedicated VR conditioner IC such as MAX9924 family) rather than relying solely on the ECU's internal VR front end for marginal-amplitude signals.
