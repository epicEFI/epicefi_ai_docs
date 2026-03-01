# GDI High-Pressure Fuel Pump Failure Diagnosis on GDI-8

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: Tera (teraflopping), Joesphan, ggurov*

## Summary

One bank of a GDI-8 module showed a dead high-pressure fuel pump (HPFP) output. Scope measurements confirmed the control signal was present on the dead channel, ruling out upstream firmware/ECU issues. Coil resistance of the pump measured approximately 0.6–0.8 ohm (leads ~0.5 ohm), suggesting the pump itself is around 0.1–0.3 ohm. The suspected failure mode is overcurrent blowing the output MOSFET (AOD482, 100 V / 32 A) in the boost converter section of the GDI board. This failure mode was reported across multiple returned units.

## Details

### Symptom

- One of two HPFP control outputs on the GDI-8 was non-responsive.
- The other bank's HPFP worked normally.
- Oscilloscope confirmed the trigger signal was present at the input of the failed channel — identical waveform to the working channel — so the failure is within the GDI board's output stage, not upstream wiring or firmware.

### Diagnosis Steps

1. **Oscilloscope check**: Probe both HPFP output channels. Working side showed a clean signal; the "dead" side also showed the same trigger coming through — meaning firmware and ECU are sending the command correctly.
2. **Coil resistance measurement**:
   - HPFP coil measured 0.6–0.8 ohm with the test leads.
   - Test leads themselves contribute ~0.5 ohm, so net coil resistance ≈ 0.1–0.3 ohm.
   - At ~12 V supply and sub-ohm coil: peak current can easily exceed 12 A; with boost voltage (PT2001 can drive up to ~65–70 V), peak current during boost phase is very high.
3. **MOSFET check**: Measure drain-to-source on the AOD482 with power off. A blown MOSFET will show a short or abnormal resistance.

### Suspected Failure Mechanism

- The HPFP is driven by a peak-and-hold driver (the PT2001 boost converter section).
- During the peak phase, the boost converter raises voltage significantly above 12 V, driving high current through the sub-ohm pump coil.
- The AOD482 MOSFET is rated 100 V / 32 A. With a coil resistance near 0.1–0.3 ohm and a boost voltage of several tens of volts, instantaneous current can exceed 32 A and destroy the MOSFET.
- Multiple GDI units were returned with the same failure mode, suggesting it is a recurring hardware reliability issue rather than a one-off event.

### Proposed Next Steps

- **Swap channels**: Move the HPFP wiring to the other (working) channel on the GDI-8 to confirm hardware fault.
- **Beefier MOSFET**: Replace AOD482 with a higher-current-rated drop-in to address overcurrent failures.
- **Check Vdrive**: Measure the boost converter output voltage to verify it is within expected range and not excessive.

### High-Pressure Fuel Pressure Behavior (Observation)

- During cranking, high-pressure rail showed a spike to approximately 36 bar, dropping back to ~9.8 bar when the starter disengages.
- The spike is believed to be caused by the intank low-pressure pump feeding the HPFP; without the HPFP actively controlling pulsewidth, the pressure shoots up at crank speed.
- The Proteus can log high-pressure fuel sensor data; the sensor in the N63TU application is still powered by the factory ECU (piggyback arrangement). Configuring the high-pressure input in TunerStudio is required to see it in logs.

### Running on Intank Pump Only (Low-Pressure GDI)

- It is possible to attempt cranking/running without a working HPFP using only the intank (low-pressure) pump.
- Key concern: cylinder compression will push fuel back out of a late-injecting GDI injector. Injection timing must be early enough (before compression stroke) to avoid this.
- At low pressure (intank ~3–5 bar vs. normal 50–200 bar GDI rail), injector flow is drastically reduced. Injector pulsewidth must be increased accordingly, but there is a hard limit from the square-law relationship between pressure and flow (see separate doc on fuel density and flow physics).
