# EFI Relay Wiring Strategy

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @Chemex-joshseek, @DCwerx, @ZeeKay, @Lysdex*

## Summary

Community members compared different relay wiring strategies for EFI-equipped vehicles, discussing how many relays are needed, what to protect with each, and how to handle accessory outputs like boost solenoids and wideband sensors. Configurations ranged from a minimum of 3 relays to 5 or more, depending on the complexity of the build.

## Details

### Minimum Relay Count

@Chemex-joshseek noted that three relays are sufficient to run a car with EFI. This represents the practical minimum for a basic standalone setup.

### Common 4-Relay Layout

@DCwerx uses four relays on all builds, daisy-chaining units as needed:

1. **Main relay** — powers the ECU and enables other relays
2. **Fuel + spark relay** — combined coil/injector power
3. **Fuel pump relay** — dedicated fuel pump circuit
4. **Fan relay** — cooling fan

### Extended 5-Relay Layout

@ZeeKay described a 5-relay setup using a GEP 72 PDM/fuse block unit (accommodates up to 280 mini relays/fuses in a compact form):

1. **ECU/Main relay** — enables all other relays
2. **Fuel pump (FP) relay**
3. **Ignition relay** — coil power
4. **Injector relay**
5. **O2 sensor relay** — allows toggling the wideband/O2 heater via relay

@ZeeKay noted that the ignition and injector relays could be combined into a single relay if space is limited; they kept them separate only because there were spare positions in the relay box.

### Additional Accessory Outputs

@Lysdex pointed out that spare ECU output pins (power-related lines) can be repurposed for accessories such as:
- Boost solenoid
- Wideband O2 sensor power/control

### ECU Fuse Protection

@Chemex-joshseek recommends installing a **2A fuse** on the ECU supply circuit. @Psychedelia questioned whether 2A is sufficient for the ECU supply, but acknowledged it is likely adequate and provides protection against wiring faults.

### Hardware Notes

- The **GEP 72** PDM/relay block was highlighted as a space-efficient solution for housing many mini relays and blade fuses in a single unit.
- @DCwerx uses modular relay blocks that can be daisy-chained to expand capacity as needed.
