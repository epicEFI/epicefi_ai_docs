# uaEFI Grounding: Sensor Ground Pin E2 and Power Ground Wiring

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @joesphan, @robb235_00916*

## Summary

This thread covers proper grounding practices for the uaEFI ECU running alongside a stock ECU. The community confirmed that pin E2 is a safe choice for sensor ground, that power grounds should go to the engine block at the same point as the stock ECU, and that sharing sensor ground via E2 does not contaminate the stock ECU's sensor ground plane as long as the power ground is properly connected. 14 AWG wire for power grounds was confirmed sufficient when no onboard wideband oxygen (WBO) sensor is used, with injectors being the main current-drawing load.

## Details

### Sensor Ground Pin Selection

Pin E2 is the recommended sensor ground connection for the uaEFI:

> "Something like pin E2 is a safe bet for sensor ground" — @joesphan
> "Yea so hookup sensor ground to pin e2" — @joesphan

### Power Ground Wiring

Power grounds for uaEFI should be connected to the engine block, ideally at the same grounding point used by the stock ECU:

> "I have the uaEFI's powergrounds going to the engine block, grounded at the exact same point as the stock ECU" — @robb235_00916
> "Ok. So I should be good there. uaEFI's power grounds go to engine block." — @robb235_00916

### Shared Sensor Ground and Multi-ECU Isolation

When running both the uaEFI and a stock ECU simultaneously, each ECU can independently sink its power into the vehicle while sharing a common sensor ground without contamination:

> "Each ecu can individually sink their power into the vehicle, yet maintain a shared sensor ground that's uncontaminated" — @joesphan

Tying uaEFI's sensor ground to E2 will not contaminate the stock ECU's sensor ground plane, provided the power ground is not disconnected:

> "Will tying uaEFI's sensor ground into E2 contaminate the stock ECU's sensor ground plane at all?" — @robb235_00916
> "No unless you have power ground disconnected" — @joesphan

### Wire Gauge for Power Grounds

Two 14 AWG wires to the engine block are sufficient for uaEFI power grounds when no onboard WBO sensor is used:

> "my uaEFI power grounds are two 14ga wires to the engine block. That awg should be sufficient, right? I'm not using onboard WBO, so no current going through uaEFI power grounds there." — @robb235_00916

The main current-drawing component in the system is the injectors:

> "I guess the only thing dumping a bunch of current for me is the injectors" — @robb235_00916

### TPS Jitter and Sensor Ground

Improper or missing sensor ground can contribute to TPS signal jitter. Connecting uaEFI sensor ground to pin E2 was explored as a potential remedy:

> "I'm wondering if connecting uaEFI sensor ground to E2 will help with some of my TPS jitters" — @robb235_00916
> "How many mv jitters? Is there correlation to jitters and running a load?" — @joesphan

Diagnosing jitter should include measuring the millivolt amplitude and checking whether the jitter correlates with engine load conditions.
