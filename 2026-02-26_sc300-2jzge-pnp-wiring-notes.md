# SC300 (2JZ-GE N/A) PNP Adapter Wiring Notes

*Source: epicEFI Discord, 2026-02-26 | Channel: 1356732771325968630*
*Contributors: @ggurov, @SanGawku, @Joshan_Lu (@joshseek_30236_07984), @ZeeKay*

## Summary

This documents a live troubleshooting session for wiring a rusEFI/epicEFI PNP adapter to a 1992 Lexus SC300 (2JZ-GE non-VVTi engine with distributor ignition). Key findings: the SC300 2JZ-GE has no MAP sensor (timing is MAP-based via distributor, fueling is MAF-based), IGT1 from the PNP connects to the distributor as coil1, crank position signal routes correctly through the SC300 PNP adapter, and the UA-EFI board was used with grounds separated to avoid VR circuit noise issues.

## Details

### Vehicle and Engine Configuration

- **Vehicle:** 1992 Lexus SC300
- **Engine:** 2JZ-GE (non-VVTi, naturally aspirated)
- **Ignition:** Distributor (single coil, not direct coil-on-plug)
- **Transmission:** 5-speed manual
- **ECU Board:** UA-EFI (UAEFI)

### Sensor Architecture (2JZ-GE NA)

The 2JZ-GE NA does **not** have a MAP sensor:
- **Timing:** MAP-based (computed from distributor advance curve; no intake MAP sensor present)
- **Fueling:** MAF-based (mass air flow sensor is the primary load input)
- Engine position/trigger signal is inside the distributor (no separate crank trigger wheel on this application)

For reference: the SC300 pinout resource referenced during this session was a ClubLexus thread covering 2JZ-GE NA/T/TT ECU pinouts — [https://www.clublexus.com/forums/sc-1st-gen-1992-2000/492459-2jzge-na-t-tt-ecu-mod.html](https://www.clublexus.com/forums/sc-1st-gen-1992-2000/492459-2jzge-na-t-tt-ecu-mod.html)

### PNP Wiring Confirmed Working

| Signal | PNP Mapping | Notes |
|--------|-------------|-------|
| IGT1 (ignition trigger) | coil1 on PNP | Goes to distributor |
| Crank position | Correct PNP pin | From SC300 distributor; routes to correct location on PNP adapter |
| Injectors | Correct PNP pins | Confirmed in place |

Connector: The SC300 main harness uses the same ECU connector family as the Supra MK4, so the UA-EFI's Supra MK4 plug is compatible.

### Crank Trigger / Trigger Wheel

The SC300 2JZ-GE uses a distributor-based trigger. There is no external crank trigger wheel. The crank position signal is sourced from inside the distributor and routes through the stock harness to the ECU connector.

Note: the 2JZ-GE crank pulley is known to develop cracks over time. If the engine is disassembled, inspect or replace the crank pulley — a failed pulley can cause loss of trigger signal.

### Known Gotchas for SC300 2JZ-GE rusEFI Conversion

- **No MAP sensor** — if the tuning strategy expects MAP as a load input, a MAP sensor must be added, or fueling must be configured as MAF-based
- **Distributor trigger** — 12/24 + 1 trigger pattern (12 teeth + 24 teeth + 1 sync tooth); no external crank trigger wheel
- **Crank pulley cracking** — inspect on teardown
- **Grounding** — separate analog/sensor grounds from power grounds on the UA-EFI board; VR circuits on the UA-EFI are not differential, making clean grounding especially important
- **VSV valves** — connector pins can be pulled out without tools; repinning connectors is straightforward if needed
- **TPS2** — the second throttle position sensor (part of the TRAC system on the throttle body) may be unplugged or have its fuse pulled on cars that have been converted to drift use; confirm its status before troubleshooting TPS-related issues
- **Supra MK4 connector compatibility** — the main ECU harness plug is the same family as the Supra MK4; a UA-EFI board with a Supra MK4 connector will plug directly into an SC300 harness

### Grounding for UA-EFI on 2JZ-GE

Joshan Lu flagged that the UA-EFI (UAEFI) board has a shared ground layout and the VR input circuits are not differential. @ggurov's recommendation: **separate the grounds** (sensor ground from power ground) and the installation will work correctly despite this limitation.
