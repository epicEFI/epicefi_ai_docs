# Counterfeit MOSFETs: Poor RDS(on) and Power Dissipation Impact

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Joesphan*

## Summary

A community member highlighted a common pitfall with counterfeit MOSFETs: their actual on-state resistance (RDS(on)) is far higher than rated, leading to significantly increased power dissipation. A fake MOSFET measured at 100 mΩ RDS(on) instead of the rated 20 mΩ will dissipate five times more heat under the same current load, potentially causing thermal failure in ECU driver circuits.

## Details

### Counterfeit MOSFET RDS(on) Problem

Fake MOSFETs sourced from questionable suppliers often have very poor RDS(on) characteristics. In a specific example shared:

- **Rated RDS(on):** 20 mΩ
- **Measured RDS(on):** 100 mΩ (5x the rated value)

This discrepancy means the device is passing as genuine but performing far below specification.

### Power Dissipation Formula

Power dissipation in a MOSFET (or any resistive element) follows:

```
P = I² × R
```

Where:
- `P` = power dissipated (Watts)
- `I` = current through the device (Amps)
- `R` = RDS(on) resistance (Ohms)

For example, at 10 A of load current:
- Genuine MOSFET (20 mΩ): P = 10² × 0.020 = **2 W**
- Counterfeit MOSFET (100 mΩ): P = 10² × 0.100 = **10 W**

The counterfeit device dissipates 5x more heat, which can overheat the component and surrounding PCB traces, cause driver failure, or damage injector/solenoid circuits.

### Practical Implications for rusEFI/epicEFI

- When sourcing MOSFETs for injector drivers, fuel pump control, or other high-current outputs, always purchase from reputable distributors (Mouser, DigiKey, LCSC from known brands).
- Measure RDS(on) with a suitable meter or component tester if parts origin is uncertain.
- Counterfeit parts are particularly common on AliExpress and similar marketplaces for popular part numbers.
- Even a seemingly working circuit may fail under sustained load if counterfeit MOSFETs are installed.
