# Kill Switch, Power Bus, and Ignition Coil Power Management

*Source: epicEFI Discord, 2026-02-20 | Channel: 1411925674498719775*
*Contributors: @mynameisdeleted*

## Summary

This thread documents how a kill switch interacts with the ECU's ignition coil control in a motorcycle/small engine application using rusEFI. The kill switch disconnects the 12V power bus from the high side of the ignition coils, rendering the ECU's ground-switching outputs ineffective. This also prevents the ECU from engaging the starter solenoid.

## Details

### ECU Ignition Architecture

In this system, the ECU controls ignition coils via **ground-switching**: the ECU output pulls the coil's low side to ground to trigger a spark. The high side of each coil is powered from the vehicle's power bus (+12V via the kill switch).

### How the Kill Switch Works

The kill switch is wired to **interrupt the +12V power bus** that feeds the high side of the ignition coils (and the starter solenoid):

> "kill switch disconnects power-buss from high side of ignition coils so the ecu's ground-switching does nothing"

When kill is pressed:
1. The high side of all ignition coils loses power.
2. The ECU's ground-switch outputs have no effect — no current flows through the primary coil winding regardless of ECU command.
3. The starter solenoid also loses its 12V feed through the kill switch, so **the ECU cannot crank the engine**.

### Voltage Rail Dropout Behavior

At engine shutdown (kill switch pressed while running):

- As RPM decreases, the alternator/generator output drops.
- The +V bus voltage falls with RPM.
- At **1200 RPM and above**, the +V rail provides reliable **14V+ high-current** supply (voltage increases with RPM).
- Below ~1200 RPM, the bus voltage becomes unreliable.
- Eventually the bus voltage hits 0 unless the **start button is held down** to maintain battery feed:

> "as rpm goes down the bus eventually hits 0 and if the start-button isn't held down, then the battery wont rescue that"

### Input Voltage Handling

To handle wide input voltage variations (particularly relevant for magneto-based or generator-fed systems):

- **1200V-rated diodes** and a **PNP switching transistor rated at 1200V** are used.
- This provides reliable switching input across a range of **6V to 1200V**.

This is relevant for ignition systems on small engines or motorcycles where the magneto can produce high-voltage spikes.

### Summary of Power Bus Consumers Through Kill Switch

| Consumer | Kill Switch Effect |
|---|---|
| Ignition coil high side | Disconnected — coils cannot fire |
| Starter solenoid | Disconnected — ECU cannot crank |
| Fuel pump, headlights, signals | Unaffected if wired direct to battery |

The fuel pump, headlights, and signals are described as operating normally from the +V rail at 1200+ RPM but may be wired separately from the kill switch path in this configuration.
