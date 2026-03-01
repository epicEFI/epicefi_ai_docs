# H-Bridge FET Gate Probing: Measuring VGS on High-Side and Low-Side MOSFETs

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: Tera (@teraflopping), Joesphan (@joesphan), fast335xi (@fast335xi)*

## Summary

Practical guidance for using an oscilloscope to measure gate drive signals on both high-side (HS) and low-side (LS) N-channel MOSFETs in an H-bridge power stage, as used in the GDI8 board. Covers correct probe placement, scope isolation requirements, and VGS limits for the AOD482 MOSFET.

## Details

**Why check both high-side and low-side FET gates:**

HS and LS FETs have different roles in the switching cycle. The LS FET source is referenced to ground, making it straightforward to probe. The HS FET source floats at the load voltage (12 V or bus voltage), so its gate is driven above supply voltage by a bootstrap or charge-pump circuit. Both gates must be verified to confirm the driver is working correctly on each side.

**AOD482 MOSFET specifications (relevant limits):**

The MOSFET used on the GDI8 board (at time of discussion) is the **AOD482** (AOSMD package).
Datasheet: https://www.alldatasheet.com/datasheet-pdf/view/485266/AOSMD/AOD482.html

- **VGS(max):** ±20 V — this is the absolute maximum gate-to-source voltage, not an operating target.
- A 7 V VGS reading on the LS FET is within normal operating range (typical N-MOSFET threshold is 2–4 V, full enhancement typically at 10 V).
- A 20 V reading appearing on an HS gate measurement is misleading if the probe ground is not referenced to the HS source — see isolation note below.

**Correct probe placement for VGS measurement:**

To measure the true gate-to-source voltage (VGS) of the high-side FET:

1. Place the **probe ground clip (black)** on the HS FET **source** pin — this is the floating switching node (the output, connected to the load).
2. Place the **probe tip (red)** on the HS FET **gate** pin.
3. The oscilloscope will display the actual VGS drive signal relative to source, regardless of what the source voltage is doing.

If instead you place the scope ground at board GND and the probe tip at the gate, you are measuring gate voltage relative to board ground — not VGS — and the reading will appear elevated by the source voltage, giving a misleadingly large number.

For the **low-side FET**, the source is at board GND, so standard probe placement (scope ground at board GND, probe tip at LS gate) gives a direct VGS reading.

**Scope isolation — avoiding shorts on floating nodes:**

When the scope is powered from a mains outlet, its safety ground (third prong) bonds the probe ground clips to earth. If the GDI8 is powered from a bench supply also connected to mains, the probe ground is shared with the board GND through the supply. Attaching this to the HS FET source (a floating switching node) effectively shorts it to earth through the scope ground conductor.

Solutions (in order of convenience):
- Use a **battery-powered scope** (no mains connection at all).
- Use a **USB scope** with the laptop running on battery and disconnected from the wall.
- Run the GDI8 from a **car battery** rather than a mains power supply — the car battery has no earth reference, breaking the common ground.
- Cut the ground prong from the scope's AC extension cord to float it from earth (works but removes a safety ground — note the risk).

**LS source measurement:**

The LS FET source should measure essentially at ground (0 V or a very small millivolt drop across the current-sense shunt). A 0.001 Ω shunt resistor in series with the LS source is used for current measurement and is not a meaningful resistance for voltage-drop purposes — it is effectively a wire.
