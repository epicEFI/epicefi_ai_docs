# GDI-8 Hardware Overview: Architecture, Controller IC, and Open/Closed Source Status

*Source: rusEFI Discord, 2025-11-20 | Channel: epicEFI-main (1356732771325968630)*
*Contributors: ggurov, Tera (teraflopping), Joesphan, andrey*

## Summary

The epicEFI GDI-8 module is physically two GDI-4 boards on a single assembly. It uses the NXP PT2001 injector driver IC rather than the MC33816 shown in older diagrams. As of November 2025, GDI-4 firmware and hardware design files are open source; GDI-8 firmware is closed source. No pre-built GDI-8 firmware binary was publicly posted at this time — only GDI-6 and GDI-4 binaries were available. To flash a GDI-8 unit, you flash two separate GDI-4 firmwares, one for each sub-board.

## Details

### Physical Architecture

- The GDI-8 is two GDI-4 modules on the same PCB, each controlling one bank (4 cylinders each for an 8-cylinder engine).
- For a BMW N63TU twin-turbo hot-V V8 application, each bank has its own high-pressure fuel pump (HPFP) and a set of 4 injectors.
- The GDI-4 schematic and hardware files are open source; the GDI-8 hardware files are closed source as of this date.

### Injector Driver IC

- The GDI-8 (and GDI-4) use the **NXP PT2001** as the injector driver, not the MC33816 that appears in some older reference diagrams.
- NXP PT2001 datasheet / product page: https://www.nxp.com/products/PT2001
- The PT2001 is a multi-output solenoid driver capable of peak-and-hold drive and high-voltage boost (up to ~65-70 V) required for GDI injectors.
- Injector output signals are a differential pair (positive and negative) connecting to the GDI injectors.

### Firmware Flashing

- No GDI-8 specific firmware binary was posted as of 2025-11-20; only GDI-6 and GDI-4 binaries were published.
- Procedure for GDI-8: flash two separate GDI-4 firmware images, one per sub-board.
- ggurov confirmed the GDI-4 firmware compiles cleanly (example build output):
  ```
  text    data     bss     dec     hex filename
  28936     176   20304   49416    c108 build/gdi4.elf
  ```
- The GDI-4 pinout reference is published at: https://rusefi.com/docs/pinouts/GDI4/

### CAN Bus Communication

- The GDI modules communicate over CAN bus and their activity ("chatter") is visible on a CAN sniffer.
- A DBC file exists for decoding the GDI CAN packets; the CAN protocol is also described in firmware source code.

### Connector Pin Size Mismatch

- The Injector Module (IM / GDI module) uses **5 mm** connector pins.
- The ECU (Proteus) uses **1/8 inch** connector pins.
- This mismatch requires an adapter or custom wiring when connecting the two modules.

### Reference Hardware for N63TU Application

- High-pressure fuel pump: Bosch HDP5
  - Datasheet: https://www.bosch-motorsport.com/content/downloads/Raceparts/Resources/pdf/Data%20Sheet_67925899_HP_Fuel_Pump_HDP_5.pdf
- Injectors: Bosch HDEV 5.1
  - Datasheet: https://www.vaglinks.com/Docs/Catalogues/Bosch-Motorsport.com_HDEV_5_1_Injector.pdf
  - Rated flow: 15.9 g/s at 100 bar
- Boost converter MOSFET (on GDI board): AOD482 — 100 V, 32 A
  - Datasheet: https://www.aosmd.com/sites/default/files/res/datasheets/AOD482.pdf
