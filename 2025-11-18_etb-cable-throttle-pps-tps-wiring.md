# Electronic Throttle Body (ETB) with Cable Throttle and PPS Sensor Wiring

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Mr. Zebra, @Joesphan, @ZeeKay, @ggurov*

## Summary

A discussion covered how to interface an Electronic Throttle Body (ETB) with a cable-actuated throttle, as well as how the Pedal Position Sensor (PPS) is used with ETB systems in rusEFI/epicEFI. Key points include: mounting an ETB and using a Honda throttle cable to connect it to the TPS sensor input, configuring the PPS with its dual-sensor pedal, and understanding how the ECU handles a failed TPS sensor on an ETB.

## Details

### Can You Use ETB with a Cable Throttle?

Yes. The approach discussed is:
1. Install an **Electronic Throttle Body (ETB)** in place of or alongside the cable throttle.
2. Use a **Honda throttle cable** to mechanically drive the ETB from the driver's pedal.
3. The ETB's own **TPS (Throttle Position Sensor)** provides the electronic position signal to the ECU.

This setup allows a cable-actuated pedal to drive an ETB while still providing proper position sensing for the ECU. Someone in the community has the specific part number for the Honda cable adapter — ask in the channel for details.

### ETB Dual Position Sensor Behavior

ETBs are equipped with **two TPS position sensors** for redundancy and safety. The rusEFI/epicEFI ECU monitors both:

- If the computer detects a problem with one sensor (e.g., out-of-range voltage, disagreement between sensors), **it shuts off that sensor's reading** and relies on the other.
- This is a safety feature to prevent uncontrolled throttle behavior from a single sensor failure.

> "etb has 2 position sensors and if computer decides one of them bork then it shuts off"

### Pedal Position Sensor (PPS) Configuration

For ETB systems, the input to the ECU should come from the **Pedal Position Sensor (PPS)** on the accelerator pedal — not from the throttle body itself:

- Hook up to the **PPS sensor**, not the throttle body TPS, as the driver intent input.
- Modern accelerator pedals with PPS typically have **two sensors** on a single pedal assembly (dual-redundant, similar to ETB TPS).
- Both PPS sensor signals should be wired to the ECU.

> "it's a pps sensor, you hook up to that instead of throttle body, and you have a pedal sensor with two sensors"

### Summary of Wiring Approach

```
Driver Pedal
  └─ PPS Sensor (x2) ──► ECU analog inputs (driver intent)

ECU
  └─ ETB H-Bridge output ──► Electronic Throttle Body motor

ETB
  └─ TPS Sensor (x2) ──► ECU analog inputs (throttle position feedback)
  └─ Cable connection to pedal (optional mechanical fallback)
```
