# GDI Injector and Fuel Pump Reverse Engineering from Stock BMW DME Board
*Source: rusEFI Discord, 2025-11-22 | Channel: 1356732771325968630*
*Contributors: @Tera (teraflopping), @Joesphan (joesphan), @ggurov (ggurov)*

## Summary

Documents the process of reverse-engineering GDI (Gasoline Direct Injection) injector pulse width, injection timing, and fuel pump control signals from a spare BMW DME board, as a prerequisite to replicating that behavior with epicEFI. Covers the measurement methodology using an oscilloscope, the Bosch custom ICs found on the DME board, the boost converter circuit, and the data parameters needed.

## Details

### Goal

To correctly implement GDI injection control in epicEFI for the BMW N63 engine, the team needed reference data from the stock DME. Three specific parameters were identified as required:

1. **Injector pulse width** — how long each injector is open
2. **Injector timing** — at what crank angle relative to TDC injection occurs
3. **Fuel pump control** — how the high-pressure fuel pump (HPFP) solenoid is driven

Joesphan stated:
> "we need to know injector pulsewidth, injector timing, and fuel pump control — what kind of stock values is it running"

### Hardware Used

- **Spare/donor BMW DME board** — removed from the vehicle, used for reverse engineering
- **Saleae logic analyzer** — for capturing digital signals (injector timing, pump control)
- **Rigol DS1054Z oscilloscope** — for capturing analog/high-voltage signals

Note: GDI injector signals are **not** simple binary signals. The high-voltage peak-and-hold waveform requires an oscilloscope rather than just a logic analyzer, as Tera noted:
> "I'm assuming this will have to be done with my scope though — because those signals aren't binary"

### DME Board Findings

Upon physical inspection of the spare DME board:
- The injector switched-side outputs route to **custom Bosch ICs** (part numbers not fully legible/logged)
- A **boost converter circuit** is present, identified by the large inductor and thick high-current traces on either side of it
- Joesphan identified this as the HV (high-voltage) circuit used for the GDI injector boost supply

Joesphan's note:
> "one of those thick wires prolly your high voltage circuit — on either side of that inductor especially — that's what's used for the boost converter"

The boost converter generates the elevated voltage (typically 65–90V) required for the fast-opening GDI injector peak current phase.

### Measurement Procedure

The planned measurement procedure to capture GDI injection reference data:

1. Run the engine on stock DME fuel control (epicEFI fuel outputs disabled, Proteus in ignition-only or observer mode)
2. Lock ignition timing to **0 degrees** in epicEFI (to use cylinder 1 spark as a known timing reference)
3. Trigger the oscilloscope on the cylinder 1 spark signal (negative edge recommended as starting point, then switch to normal trigger mode)
4. Simultaneously measure:
   - **Injector 1** signal (voltage waveform showing peak-and-hold pulse)
   - **Fuel pump** control signal (HPFP solenoid drive)
5. The cylinder 1 TDC spark at 0° provides the timing reference to calculate injection angle relative to TDC

Joesphan summarized the setup:
> "Timing 0, trigger in cylinder 1 spark. Measure injector 1, measure pump"
> "Nah just get a log of the injector stuff and find a trigger reference (tdc or something)"

### Oscilloscope Trigger Setup (DS1054Z)

For anyone unfamiliar with setting up oscilloscope triggering for this measurement:
1. Connect probe to channel 1 on the injector or spark signal
2. Set trigger mode to **Auto** first to get a stable view
3. Adjust the trigger level to the midpoint of the signal
4. For injector signals, trigger on **negative edge** (GDI injectors pull low when firing)
5. Switch to **Normal** trigger mode once level is set — this locks the time axis to trigger point 0

### Why Limp Mode is Acceptable for Initial Data

Tera's vehicle was in limp mode during this session. Joesphan confirmed that limp-mode data is a useful starting point:
> "limp mode is somewhere to start — from there we can try changing timing see what it does to afr — change pw etc."

ggurov added that running data will also be "more flat without compression stuff" (i.e., without VVT and variable compression effects active, the data is cleaner for baseline reference).

### Relationship to GDI-8 Module

The data captured from the stock DME is intended to inform the configuration of the **GDI-8** external hardware module (see N63 bringup doc). The GDI-8 acts as a passthrough for GDI injectors and drives the HPFP solenoid, but it requires knowing the correct pulse widths and timing angles to replicate stock behavior under epicEFI control.
