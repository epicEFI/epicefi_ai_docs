# Custom ECU Harness Wiring: Wire Gauge Selection and Sourcing

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @joesphan, @Alice*

## Summary

A practical discussion on building a custom engine wiring harness from scratch for a rusEFI / epicEFI installation. @joesphan provided specific wire gauge recommendations, sourcing advice (buying spools of automotive-rated wire from Amazon rather than pre-made harness kits), and connector recommendations. The conversation also touched on using factory wire from a parts car, connector options, and the relative merits of building a harness versus buying a universal kit like Painless Wiring.

## Details

### Wire Gauge Recommendations

For a typical custom ECU harness build, the following gauges are commonly needed:

| Application | Gauge |
|---|---|
| Fuel pump power | 10 AWG |
| General sensors and signals | 20 AWG |
| Injectors, solenoids, moderate-current outputs | 16 AWG |

- **10 AWG** is specifically recommended for the fuel pump power circuit due to current draw
- **20 AWG and 16 AWG** are the two gauges most commonly needed throughout the rest of a harness

### Wire Sourcing

@joesphan recommended buying wire in spools from Amazon rather than using pre-built harness kits or pulling wire from a donor vehicle:

- Buy spools of automotive-rated primary wire
- Example: approximately 600 ft of wire was used for a full MR2 harness build
- Wrap with Harbor Freight electrical tape — described as OEM-rated and what manufacturers themselves use
- Amazon link referenced: https://a.co/d/aYytgUN (primary wire spool) and https://a.co/d/53tHub5 (additional supply)

Pulling wire from a parts car is a viable option but comes with caveats:
- Wire gauge compatibility must be verified
- Insulation age and condition is unknown
- @joesphan's recommendation was to buy new spools rather than reuse old wire

### Connectors

@joesphan recommended Hiltech (or similar branded) automotive connectors:

- Referenced link: https://a.co/d/8UmkuZl
- Not waterproof but are sturdy and acceptable for under-hood use in most applications
- "Not falling apart is good enough" for non-exposed locations

He specifically advised **against** using Painless Wiring harness kits.

### Avoiding Pre-Made Kits (Painless Wiring)

The recommendation was to avoid Painless Wiring universal harness kits. Instead, buying wire in bulk and building from scratch:
- Produces a cleaner, correctly-routed harness
- Takes 1-2 weekends of work but results in a better fit
- Is cost-effective when buying spools of wire

### Additional Notes

- A label maker is recommended for harness organization and traceability
- Sensors requiring external purchase for a full install include: MAP sensor, wideband O2 controller (e.g., 14Point7), and a proper crimping tool
- @joesphan offers a small dealer discount on some sensors/components for community members

### DB-37 Connectors for ECU Interface (ggurov tip)

In related discussion, @ggurov mentioned that a convenient "instant harness" approach for rusEFI/epicEFI boards using DB37 connectors is to purchase a pre-molded DB37 cable (male-to-female), potentially cutting the top connector in half or using the existing pigtail, as a starting point for the harness. The board referenced uses the MegaSquirt 3 / MS3X pinout with DB37 connectors (same as MS3Pro base connector). This provides a molded connector in whatever cable length is needed, though wire gauge may be on the lighter side for high-current circuits.
