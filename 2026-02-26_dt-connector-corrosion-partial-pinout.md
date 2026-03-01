# Deutsch DT Connector Issues: Corrosion, Worn Sockets, and Partial Pin Population

*Source: epicEFI Discord, 2026-02-26 | Channel: 1411925674498719775*
*Contributors: @Lysdex, @Joshan_Lu, @ZeeKay, @Immrpablo*

## Summary

Multiple community members encountered problems with 2-pin Deutsch DT (and similar automotive) connectors: worn or corroded sockets that no longer retain the bulb/terminal, and stock corrosion that causes contact failure. Workarounds include using generic pigtail replacements and populating only the required pins of a larger DT connector when fewer circuits are needed.

## Details

### Worn/Corroded Deutsch 2-Pin Sockets

**Symptoms:**
- The bulb or terminal no longer seats or locks in the old socket.
- Severe corrosion on the contact surfaces, with one or more contacts physically breaking.
- Despite broken contacts, circuits may continue to function (intermittently or normally) in some cases.

**Root cause:** Deutsch sockets (and similar connectors) are designed to be replaceable — the socket/receptacle can be swapped out from the connector body when it wears out. However, replacement sockets or full connector assemblies are not always on hand.

**Workarounds:**

1. **Generic pigtail replacement (preferred):** Purchase a generic 2-pin replacement connector with pre-attached pigtail wires. No spade block or separate terminal insertion is needed — just splice the pigtail wires into the existing harness.
   - One reported source for a compatible 2-pin Honda/Nissan-style plug: [BattleBorn Speed Shop Honda K-Series VTEC solenoid connector plug](https://www.battlebornspeedshop.com/products/honda-k-series-vtec-solenoid-connector-plug-2-pin-k20-k24) — reported as visually similar to the socket in question.
   - Also reviewed: [HangTight K-Series connector kit](https://hangtight.io/products/k-series-connector-kit) — noted as similar to the VTEC plug commonly found in Honda harnesses.
   - As one user noted: "it's just a 2 pin plug, so they will probs use it in a bunch of places."

2. **Use upstream connector:** If a 6-pin DT connector is available elsewhere in the harness with unused cavities, re-route the circuit through it rather than repairing the damaged 2-pin body.

### Populating Only Some Pins of a DT Connector

When using a larger DT connector (e.g., 6-pin DT06/DT04) for a circuit that only needs 2 wires:

- You can **populate only the cavities you need** and leave the rest empty (with cavity plugs for weatherproofing).
- The rusEFI/epicEFI software allows enabling or disabling specific pin functions, so unused positions do not need to be wired.
- Suggested approach: use the remaining cavities of an already-present 6-pin DT connector on the harness rather than adding a separate 2-pin connector.

### General Tips

- Inspect connector sockets for corrosion any time you have intermittent signal issues — corroded contacts can still pass current but add resistance and drop voltage under load.
- When replacing connectors on existing harnesses, pigtail-style replacements (where the wires are pre-crimped) are much faster to install than buying bare shells and re-crimping terminals individually.
