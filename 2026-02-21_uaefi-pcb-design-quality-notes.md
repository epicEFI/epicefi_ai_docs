# uaEFI PCB Design Quality Notes: Via Current Rating and Single-Sided Layout

*Source: epicEFI Discord, 2026-02-21 | Channel: 1356732771325968630*
*Contributors: @jeybee, @GGGI (.gunadi), @ogalic, @ggurov*

## Summary

A discussion on the electrical design quality of the uaEFI board raised concerns about via current capacity, particularly for high-current loads like an electronic throttle body (ETB). Guidelines for via sizing were established, and suggestions were made for future board designs to reduce production cost while maintaining reliability.

## Details

### Via Current Capacity — Rule of Thumb

For PCB vias carrying current in an automotive engine bay environment:

- **Minimum: 1 via of 0.33 mm diameter per amp of load.**
- A Bosch ETB (electronic throttle body) has a **nominal current draw of 5–8 amps**, requiring a minimum of **5 vias** to survive daily use in a hot engine bay.
- Specific uaEFI board data: 1.6 mm board thickness, 25 micron plating — measured at **1.6–1.8 A at 40°C temperature rise** per via.
- A 13 mil (0.33 mm) via carries approximately **3 A** (per ogalic's notes).
- The uaEFI board was noted as being rated at 1–2 A max per path for the ETB circuit — this was flagged as potentially problematic for long-term reliability.

### Thermal Reliability Concerns

- The board is considered to be **"on the limit"** for automotive use, which is not an acceptable design margin for automotive-grade hardware.
- Known failure modes include: MOSFET burn, Bluetooth module power draw issues (flagged as a red flag for design margin).
- Over time, vibration and thermal cycling can lead to **solder joint cracking** — MTBF estimated at **months to a year** for daily use.
- Workaround suggested for marginal vias: scrape soldermask, solder a wire bodge to the carrier.
- The board was described as more of a **"development board"** than a production automotive ECU.

### Single-Sided PCB Design Recommendation

To reduce production costs for future hardware designs:

- Placing components on only **one side of the board** significantly lowers manufacturing costs (avoids double-sided reflow or pick-and-place passes).
- Response from hardware developer (GGGI): **both sides are acceptable**, as long as:
  - Not too many components are placed on the second side.
  - Components on the second side are easy to **hand solder**.
- Priority is keeping secondary-side components minimal and hand-solderable for low-volume production.

### DC2 Output Usage

- `jeybee` questioned whether anyone was using the **DC2 output** on the uaEFI board.
- `ggurov` confirmed using it for a **stepper motor** at some point.
- See also: separate thread on DC1 vs DC2 for VW EWG control.
