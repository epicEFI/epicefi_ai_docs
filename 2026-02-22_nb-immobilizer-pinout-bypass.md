# Mazda NB Immobilizer Operation, ECU Pinout Compatibility, and Aftermarket ECU Bypass

*Source: epicEFI Discord, 2026-02-22 | Channel: 1464230782905352261*
*Contributors: @MartyMotorsports, @Neos6443*

## Summary

The Mazda NB (Miata) immobilizer system uses a three-way handshake between the key, a separate immobilizer unit, and the ECU. On aftermarket ECUs such as epicEFI or Speeduino, the immobilizer signal wire can simply be left disconnected — the car will run normally but the dashboard immobilizer warning light will flash. The '01+ 1.6L NB and '01–'02 1.8L NB share an identical ECU connector pinout, which has implications for ECU swapping between these variants.

## Details

### Immobilizer System Architecture (Mazda NB)

The NB immobilizer operates as a three-node system:

1. **Key** — contains a transponder chip; communicates with the immobilizer control unit.
2. **Immobilizer unit** — reads the key transponder and signals the ECU.
3. **ECU** — receives an enable-fuel signal over a single wire from the immobilizer unit.

If all three nodes are satisfied, the engine cranks and runs normally. If the key is not recognized, the car will not crank (confirmed by Neos6443: a newly cut key without proper transponder programming prevented cranking entirely).

### Regional Variant Differences

- **US-market NB**: The immobilizer communicates directly with the ECU; if the key is not recognized, fuel is cut after approximately 3 seconds of running.
- **UK/European NB**: The immo unit sits between the key and the ECU; the ECU receives a single "enable" signal. The car will not crank at all if the immo is not satisfied (rather than cutting fuel after cranking).

### Bypass with Aftermarket ECU (epicEFI / Speeduino)

- With an aftermarket ECU, the immobilizer wire to the ECU has no functional effect on fuel delivery — the aftermarket ECU does not interpret the immo signal.
- The single wire from the immobilizer unit to the stock ECU connector can simply be left unconnected on the aftermarket ECU side.
- Result: the car starts and runs normally, but the immobilizer warning light on the dashboard will continue to flash (cosmetic only).
- Neos6443 confirmed this bypass approach works on a UK-spec NB with Speeduino.

### Stock Alarm / Factory Security System

- NB Miatas do not come with a full factory alarm; they have a panic button (horn trigger) but no proper immobilizer-integrated alarm from the factory.
- Some cars have a dealer-fitted Mazda-branded alarm that does include an ignition/crank cut: it cuts the starter circuit if the car is locked using the alarm while occupants are inside, or if the locks are not disarmed through the proper sequence.
- This dealer alarm is distinct from the immobilizer and wires into the starter/crank circuit independently.

### ECU Pinout Compatibility: NB 1.6 vs NB 1.8

- The **'01+ 1.6L NB** ECU connector pinout is **identical** to the **'01–'02 1.8L NB** ECU pinout.
- This means a 1.6 ECU can physically plug into a 1.8 harness (and vice versa), though the fueling maps will be mismatched by the 0.2L displacement difference — the 1.6 ECU will inject less fuel than needed for the 1.8.
- The immobilizer system on the two variants may differ; swapping ECUs across these models may trigger immobilizer issues depending on regional specification.
- MartyMotorsports was investigating the possibility of running a 1.6 NB ECU on a 1.8 engine, noting the ROM chip inside the NB2 ECU as a potential target for dumping/reflashing.
