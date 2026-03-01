# Gear Position Sensing: Mechanical Selector with Hall Sensors and Switch-to-Ground Methods
*Source: rusEFI Discord, 2025-11-19 | Channel: 1356732771325968630*
*Contributors: @ggurov (ggurov), @Joesphan (joesphan), @kostasl (kostasl0737)*

## Summary

Describes two approaches to gear position sensing used on motorcycle/small-engine applications: a hall-sensor-on-sprocket approach for larger bikes with a mechanical selector shaft, and a simple digital switch-to-ground approach common on small-cc bikes.

## Details

### Hall Sensor on Mechanical Selector Shaft

On some motorcycle setups, the gear position is determined mechanically:
1. A **cam on the up/down gear selector** rotates a shaft (similar to a thumbwheel mechanism).
2. That shaft is connected to the actual gear selector mechanism.
3. A **hall sensor reads a sprocket** (with screws or teeth) attached to that shaft to determine angular position, and therefore the current gear.

This gives an analog or multi-count digital representation of gear position tied directly to the physical gear selector shaft rotation.

### Switch-to-Ground (Digital) Gear Position (Small-CC Bikes)

For small-displacement motorcycle engines (small cc bikes), a full analog gear position sensor is often not present. Instead:
- A **simple on-off switch to ground** indicates each gear position.
- A typical **4-gear gearbox** uses **5 wires**: one common and one per gear state (or per switch).
- Each wire is either pulled to ground or open to signal the current gear.

This is simpler and cheaper than an analog sensor but provides discrete gear readings rather than a continuous position value.

### Context for rusEFI/epicEFI

When configuring gear detection in rusEFI/epicEFI for a motorcycle application:
- Identify whether the bike uses an analog sensor or digital switch-to-ground wiring.
- For hall-sensor setups, configure the sensor as a speed/position input.
- For switch-to-ground setups, configure the appropriate number of digital inputs and map them to gear positions.
