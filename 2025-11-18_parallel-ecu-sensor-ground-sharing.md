# Parallel ECU Sensor Ground Sharing (uaEFI + Stock ECU)

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Robb235, @Joesphan*

## Summary

@Robb235 described a piggyback/parallel wiring setup where uaEFI (and a planned epicEFI) runs alongside the stock ECU, reading factory sensor signals via a patch harness. The key question raised was whether the sensor ground planes of the two ECUs should be tied together to share a common ground reference.

## Details

### Setup Description

- **uaEFI** is wired in parallel with the stock OEM ECU
- A **patch harness** allows uaEFI to tap into factory sensor signal wires
- Factory sensors (TPS, CLT, IAT, MAF, etc.) receive their **power and ground from the stock ECU**
- uaEFI only reads the **signal wires** from these sensors — it does not power them
- Sensors not present from the factory (MAP sensor, flex fuel sensor) report **exclusively to uaEFI**

### The Ground Question

@Robb235 asked whether the uaEFI sensor ground plane should be tied to the stock ECU's sensor ground plane so both systems share the same ground reference.

@Joesphan (an electrical engineer) engaged with the question and encouraged @Robb235 to elaborate further on the specifics of the setup before giving a definitive recommendation.

### Technical Considerations

When two ECUs share sensor signals in a parallel arrangement, ground reference integrity is critical:

- If the two ECUs have **separate ground references**, a voltage potential difference between them can cause sensor readings to differ between the two ECUs, or introduce noise on signal lines.
- Tying sensor grounds together ensures both ECUs see the same reference voltage for shared sensors.
- However, care must be taken to avoid **ground loops**, especially if the two ECUs are also grounded to the vehicle chassis at different points.

This thread was the start of a longer troubleshooting/design conversation; the definitive wiring recommendation was not yet reached in this excerpt.
