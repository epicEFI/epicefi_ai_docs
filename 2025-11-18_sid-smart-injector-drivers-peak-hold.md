# Smart Injector Drivers (SiD): High-Voltage Peak-and-Hold Injector Control

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @joesphan, @kzairsoft (BroKingscooters), @ggurov, @Alice*

## Summary

A discussion on the epicEFI "Smart Injector Drivers" (SiD) feature, which drives fuel injectors at significantly elevated voltage to reduce injector deadtime. The concept is analogous to how stepper motor driver boards overdrive nominally 5V steppers at 40-60V to achieve much faster step response. By driving a 12V fuel injector at voltages up to ~50V in a peak-and-hold pattern, the injector opens substantially faster, effectively eliminating the deadtime that conventional injector drivers must compensate for. The discussion also covers injector selection, the question of low-impedance vs. high-impedance injectors with SiD, and practical advice on sourcing high-flow injectors.

## Details

### What Are Smart Injector Drivers (SiD)?

SiD is a feature implemented in the epicEFI hardware by @joesphan. The name expands to "Smart Injector Drivers." Key characteristics:

- SiD is an optional feature; conventional lower-voltage injector drivers are also available
- SiD drives injectors at elevated voltage (peak phase) to open them rapidly, then drops to a lower hold voltage to keep them open
- This is the same operating principle as "peak-and-hold" (P&H) injector drivers, but extended to operate at higher peak voltages than typical P&H implementations
- The stated peak voltage is approximately 50-55V (not 70V as initially questioned — 70V is not the operating voltage)

### The Physics: Why Overdrive Works

@kzairsoft explained the principle clearly using the stepper motor analogy:

> "3D printers use 5V stepper motors. At 5V they are slow and lethargic. Drive them at 60V pulsed and they wake up a lot. The step time is reduced by orders of magnitude and has more authority. [Similarly,] a 12V injector run at up to 50V — now the injector time to fully open is much faster."

The injector coil is an inductor. Higher voltage drives current through the coil faster (di/dt = V/L), so the solenoid plunger opens sooner after the trigger signal. The key benefit:

- **Injector deadtime** is the interval between when the injector receives a signal and when it actually begins to open. Conventional drivers at 12V have measurable deadtime that must be calibrated and compensated in the fuel table.
- With SiD, the rapid current rise from the high-voltage peak dramatically reduces this deadtime — @joesphan's claim is that deadtimes "don't exist anymore" with SiD.

Reference for underlying theory: https://www.monolithicpower.com/learning/resources/high-performance-solenoid-drives

### Injector Type Compatibility

**High-impedance (saturated) injectors:**
- Recommended for use with SiD
- Can be sourced at high flow rates (e.g., 900cc+) at reasonable cost
- Example: Ford E350 remanufactured injectors, part number `0 280 158 193`
  - Flow: approximately 900cc at 4 bar fuel pressure
  - Available in sets; within ~1.5% flow rate from factory (OEM spec is ~10%)
  - Can be decapped (removing the flow-limiting cap) to increase flow further

**Low-impedance (peak-and-hold) injectors:**
- @joesphan advises against using low-Z injectors with SiD
- Reason: high-Z injectors that flow sufficiently are available cheaply, making low-Z unnecessary in most applications

### Decapping Injectors for Higher Flow

OEM injectors with a restrictor cap can be decapped to increase flow rate and improve atomization:

- Removing the cap improves atomization (finer spray pattern)
- The Nissan Sentra and similar-era WRX injectors are noted as being among the easiest to decap without seal damage
- Ford injectors also decap acceptably
- Dodge Viper injectors flow ~1300cc decapped but are difficult to decap without ruining the seal
- LS-series (GM) injectors run at 4 bar stock; flow figures quoted for them assume 4 bar fuel pressure

Per-cylinder injector trim (individual fuel table offset per cylinder) can compensate for small flow variances between individual injectors after binning.

Resources:
- Decapping guide: https://sites.google.com/site/sloppywiki/how-to-section/decap-factory-fuel-injectors
- Injector part number lists: https://ls1tech.com/forums/forced-induction/1892571-1000cc-decapped-injectors-3.html
- Video demonstrating decapped injector spray improvement: https://www.youtube.com/watch?v=KTJh6yWlKfQ (Nissan Sentra injectors, ~800cc)

### Peak Injector Voltage

The injector peak voltage in the SiD implementation:

- @kzairsoft asked why 70V was specified; @joesphan and @ggurov clarified the peak is approximately 55V, not 70V
- The SiD designation and peak voltage are specific to the epicEFI implementation
- A wiki page documenting SiD in detail was planned by @joesphan but not yet published at time of discussion

### Servo / Linear Actuator for Wastegate Control

In the same conversation, @Alice raised the question of adding a servo or linear actuator for turbocharger wastegate control. Notes from the discussion:

- Linear actuators for wastegates require significant current; epicEFI's on-board drivers may not be sufficient alone
- An external H-bridge driver can be added to handle the current requirement
- Stepper motor-based actuators are also viable with appropriate external driver boards
- The epicEFI board supports approximately 49A maximum output (referenced in context of external driver use)
- Bosch makes a motorsport-spec electronic throttle body (ETB) for approximately $100 AUD that is also relevant for boost control applications
