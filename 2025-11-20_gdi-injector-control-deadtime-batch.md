# GDI Injector Control: Deadtime, Batch vs Sequential, and Fuel Delivery Constraints

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov (@ggurov), Tera (@teraflopping), Joesphan (@joesphan), Robb235 (@robb235_00916), MykkH (@mykkh)*

## Summary

Extended discussion of Gasoline Direct Injection (GDI) support in epicEFI, covering the fundamental incompatibility of batch injection with GDI, deadtime behavior differences between port and direct injection, injector sizing, and the reduced but time-constrained fuel delivery window. GDI must run sequential injection only. The epicEFI deadtime tuner tool's applicability to GDI is uncertain and under investigation.

## Details

**GDI injector control menu location:**
- In TunerStudio: `Expert/Extra > GDI > GDI Injector control`
- As of the 2025-11-16 build, the menu was found empty (hidden by a `ts_show_gdi_low_level` flag).
- The GDI control fields exist in the firmware; they are gated by `@@if_ts_show_gdi_low_level`.
- Example hidden field: `field = "MC33816 rstb", mc33816_rstb@@if_ts_show_gdi_low_level`
- ggurov was re-enabling the GDI menus for the Proteus F4 build in response to this discussion.

**GDI cannot run batch injection:**
- Batch injection fires two injector pulses per engine cycle (one per 360 degrees).
- In port injection, the extra "wrong phase" pulse lands on a closed intake valve — harmless.
- In GDI, fuel is injected directly into the combustion chamber. A pulse on the wrong stroke would inject fuel during the exhaust stroke, sending unburned fuel out the exhaust.
- Therefore: **GDI must use sequential injection only** (one pulse per engine cycle per cylinder, timed to the correct stroke).
- Confirmed by both Robb235 and ggurov: "GDI can't run batch" — "like wastespark's gonna light some shit on fire."

**Sequential vs batch injection — deadtime implications:**
- Sequential = 1 injection event per engine cycle per cylinder.
- Batch = 2 injection events per engine cycle per cylinder.
- For a given fuel quantity:
  - Sequential: applies 1x deadtime correction.
  - Batch: must execute two pulses, each with deadtime overhead, to deliver the same total fuel mass.
  - Batch therefore incurs 2x deadtime cost to deliver the same fuel amount.
- The epicEFI deadtime tuner tool works by alternating between batch and sequential modes (every ~100 cycles) and comparing AFR:
  - If deadtime is calibrated correctly, AFR should remain identical whether batch or sequential is used.
  - The AFR difference between modes reveals deadtime error.

**Deadtime tuner tool compatibility with GDI — unknown:**
- Since GDI cannot run batch injection, the batch/sequential alternation approach of the deadtime tuner cannot be applied directly to GDI.
- Robb235: "Wouldn't work, would it? Cause no longer spraying fuel onto the back of a closed valve?"
- GDI deadtime characterization requires a different methodology — under investigation.

**GDI injector deadtime characteristics:**
- GDI injectors operate against high fuel pressure (several hundred to several thousand PSI).
- Despite this, GDI injectors have a *smaller* deadtime compared to port injectors (MykkH, confirmed via external research).
- This is counterintuitive given the high backpressure but is an established characteristic of GDI injector design.
- Cylinder temperature becomes more critical with GDI because it directly affects charge properties at the time of injection into the cylinder.

**GDI injector sizing vs port injection:**
- GDI injectors do not necessarily need to be larger than their port injection counterparts.
- GDI systems achieve efficient fuel delivery through: high fuel pressure, precise timing, and often multiple injection events per cycle.
- GDI can achieve good mixture quality with less total fuel mass due to improved atomization from direct injection into the cylinder (Tera).
- However, the time window available to deliver that fuel is significantly smaller in GDI vs port injection — the injector must complete delivery within the compression stroke window (Robb235).

**BMW Valvetronic (related — brushless motor control for variable valve lift):**
- Later BMW Valvetronic systems use a brushless DC motor per bank.
- Hardware requirements per bank: 1x 3-phase motor driver + 5x Hall sensor inputs.
- For a V8 (two banks): 2x 3-phase drivers + 10x Hall sensors total.
- Earlier Valvetronic used a brushed DC motor (simpler to drive).
- Full Valvetronic control is a future goal for epicEFI; not yet implemented.

**GDI hardware support:**
- The epicEFI firmware supports the MC33816 GDI driver chip (NXP/Freescale).
- The Proteus ECU is confirmed compatible with GDI operation via epicEFI firmware.
- Maximum cylinder count for current GDI hardware: 8 channels (sufficient for V8; R8/Huracán V10 GDI would require a different approach).
