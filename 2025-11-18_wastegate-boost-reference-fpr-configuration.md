# Wastegate Reference, Boost Tap, and Fuel Pressure Regulator Configuration

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat-2*
*Contributors: @Robb235, @Mr. Zebra*

## Summary

Discussion of how to correctly reference the wastegate actuator and fuel pressure regulator (FPR) on a turbocharged engine fitted with an intercooler. Key points: the wastegate actuator should reference boost pressure from the compressor housing (pre-intercooler) to see full compressor outlet pressure; the FPR on a stock-injector setup can use a boost tap (post-intercooler) for a 1:1 fuel pressure rise with boost; and the FPR reference should be atmosphere rather than manifold pressure to avoid lean conditions under vacuum. @Robb235 is also running uaEFI in parallel with a stock ECU, with uaEFI handling fueling and ignition only.

## Details

### Wastegate Actuator Reference Point

- Referencing the wastegate actuator to the compressor housing (pre-intercooler) lets the actuator see the full compressor outlet pressure and opens at the correct set point.
- **@Mr. Zebra**: "if u want slightly less boost, reference the stock actuators to the comp housing." (This reduces effective boost because the actuator sees higher pressure — pre-intercooler — and opens the wastegate earlier.)
- **@Robb235**: "Yeah, have wastegate reference boost right off the compressor housing. The manifold will see less boost after it goes through the intercooler." This confirms the pressure drop across the intercooler means the intake manifold sees less boost than what is measured at the compressor housing.

### Fuel Pressure Regulator Reference — Atmosphere vs Manifold

- **Stock ECU behavior (@Robb235)**: "Stock ECU had FPR reference atmosphere. If it reference manifold, it would be too lean in vac." A manifold-referenced FPR lowers fuel pressure under vacuum (part-throttle / idle), reducing injector differential pressure and causing lean conditions. The stock strategy avoids this by referencing atmosphere.
- For a boosted application with stock injectors, @Robb235 uses a boost tap (post-intercooler) on the FPR to achieve a 1:1 fuel pressure rise as boost increases: "I have a boost tap for my fuel pressure reg. I needed that 1:1 rise in fuel pressure as boost increased on stock injectors."

### Combining Both References

@Robb235 described his final configuration:

- **Wastegate reference**: boost tap post-intercooler (same tap as FPR was using).
- **FPR reference**: atmosphere.

"But now I need to put my waste gate reference onto the boost tap (which is post intercooler) and have my FPR reference atmosphere."

This arrangement:
1. Keeps the wastegate referenced to manifold-equivalent boost so it opens at the correct pressure relative to what the engine sees.
2. Keeps the FPR at atmospheric reference, avoiding lean-under-vacuum issues while still using a boost tap for rising-rate compensation at positive boost.

### uaEFI Running in Parallel with Stock ECU

- **@Robb235**: "uaEFI is in parallel to my stock ECU. uaEFI handles all things fueling and ignition. Stock ECU handles most everything else."
- This parallel integration pattern allows uaEFI to take over fueling and ignition while leaving chassis/body functions, idle control, and other peripherals to the OEM ECU.

### D16 Wastegate Sizing Note

- **@Chemex-joshseek**: On the Honda D16 in a turbo application, the wastegate must be large enough to avoid building excessive back-pressure in the turbo manifold. The concern is not only boost creep but manifold restriction that can choke engine breathing: "Well valves aren't as great on the d16 you have to make sure your waste gate is big enough so you don't build up too much restriction in the turbo manifold not so much for boost creep but more so of suffocating the engine."
