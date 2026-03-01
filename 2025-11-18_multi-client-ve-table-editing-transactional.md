# Multi-Client VE Table Editing: Real-Time Collaborative Tuning

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat (1356732771325968630)*
*Contributors: @ggurov, @Mitch*

## Summary

The discussion covers a proposed (and partly implemented) architecture for allowing multiple tuning software clients to connect to a single ECU simultaneously, with real-time table edits visible to all connected users. Mitch describes a transactional change model where cell updates are only reflected in the tuning UI after the ECU confirms the change, and that confirmation is broadcast to all connected clients.

## Details

### Real-Time Collaborative VE Table Editing

ggurov described the desired capability: two or more people editing the same VE table simultaneously, with each person's changes appearing in real time for all other users:

> "i.e. 2 people can be editing the same VE table, and updates will show up realtime for them" — @ggurov

This is contrasted with conventional ECU software where a second connected client may not see changes made by a first client, or where conflicting writes can cause inconsistent table states.

### Transactional Change Model

Mitch explained how his implementation handles this:

> "Cell value changes are transactional, meaning that the change is only reflected on the tuning software if the ECU says the change has been made. Same thing would be used to relay cell value changes to all tuners, and the owner of the ECU while they have the laptop in front of them" — @Mitch

Key properties of this model:
- A client proposes a cell value change.
- The ECU applies the change and sends a confirmation.
- Only upon receiving the ECU's confirmation does the tuning software update its displayed value.
- The same confirmation event is broadcast to all other connected tuning clients, keeping their tables in sync.
- The ECU owner with a direct (local) connection also receives these broadcast updates.

### Why This Matters vs Traditional Dual-Client Setups

ggurov noted the difference from current tuning software that is not designed for multi-client scenarios:

> "compared to two people being connected to an ECU that's not aware of dual tuning clients like that" — @ggurov

Without a transactional broadcast mechanism, two simultaneous TunerStudio (or equivalent) clients writing to the same ECU can produce race conditions, silently overwrite each other's changes, or display stale data.

### Practical Use Cases

- Remote tuning sessions where a tuner and a car owner both need to see live changes.
- Collaborative dyno tuning with a driver in the car and a tuner at a laptop.
- Future remote/cloud-assisted tuning scenarios.
