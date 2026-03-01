# GDI V8 First-Start Bringup: Diagnosis and Tuning Session Notes

*Source: rusEFI Discord, 2025-11-21 | Channel: epicEFI dev/support*
*Contributors: Tera, ggurov, Joesphan, SanGawku*

## Summary

This documents a live bringup session for a V8 GDI engine running epicEFI firmware. The engine was struggling to start and sustain idle. Key findings: (1) the engine ran briefly on FOME firmware but not epicEFI, pointing to an ignition/timing difference between the two; (2) at only 1.8ms injector pulse width the engine was severely lean — it required approximately 18ms PW to sustain combustion on low-pressure fuel pump only; (3) the trigger initially syncs 180 degrees out before correcting itself; (4) a methodical diagnosis sequence was established using a logic analyzer and timing light.

## Details

### Engine Configuration

- **Engine:** V8, GDI (Gasoline Direct Injection)
- **Firmware under test:** epicEFI (with comparison testing against FOME)
- **Firing order:** 1-5-4-8-6-3-7-2 (verified correct in TunerStudio and confirmed by logic analyzer)
- **Crank offset that previously allowed the engine to run:** 420 degrees
- **Cylinder identification issue:** Cylinder 8 was found non-functional (dead); located on bank 2, cylinders 5–8, driver's side

### Fuel System

**HPFP (High-Pressure Fuel Pump):**
- Cam-driven mechanical pump — spins at a fixed rate relative to the cam, with a solenoid controlling how much fuel is captured per stroke.
- Fuel pressure is **different per bank** for HPFP.
- LPFP (low-pressure fuel pump) pressure is shared/equal across banks.
- LPFP was hotwired during initial bringup.
- Stock injector flow rate: approximately **15.9 g/sec** at **100 bar** operating pressure.

**Injector pulse width observations:**
- Previous day logs showed **1.8ms PW** — insufficient fuel, engine would not sustain combustion.
- At 18ms PW (running on FOME / low-pressure only), engine exhibited a lean condition — indicating the HPFP was not yet contributing adequate pressure and the injectors need very high duty cycle when running on LPFP only.
- ggurov: "so it does need crazyfuckingtown pw to run on low pressure pump."
- Joesphan suggested a pressure:flow table would be needed for proper fuel control rather than a simple PW limit.

**Injector coding (serialized flow rate):**
- BMW-style injectors have a flow rate coded to their serial number, stored in the DME EEPROM (not in the main flash bin).
- The injector coding is read from EEPROM at startup and written to RAM during the initialization function.
- The INJAD function in the factory firmware contains both multiplicative index data and additive time offset per cylinder.

### Trigger / Sync Behavior

- The trigger system initially syncs **180 degrees out of phase**, then self-corrects.
- Sparking does not begin until full sync is achieved.
- The crank trigger offset that allowed the engine to previously run: **420 degrees**.
- Wasted spark mode was disabled as part of the debugging process.
- Backfires into the exhaust (not intake) suggested ignition was firing at the wrong point in the cycle (exhaust stroke instead of compression).

**Cranking timing notes:**
- Joesphan: "crank timing is backwards logic — if starter slows down while catching, you gotta decrease timing."
- Advancing cranking timing too far produced kickback; reducing it produced gentler cranking but still misfires.
- Suggestion to advance cranking timing to 20 degrees (from 10 degrees) was tried but did not resolve the issue.

### Diagnosis Workflow

**Step 1: Verify spark**
- Use a timing light to confirm spark is occurring at approximately the correct crank position.
- Alternatively: use a phone camera at high frame rate to observe the timing mark without a second person.
- Pull spark plugs and check condition:
  - Sooty/dry = combustion occurring but fuel delivery or mixture issue
  - Wet = no combustion, flooding

**Step 2: Verify fuel delivery**
- Spray starting fluid or fuel into the intake manifold to confirm the engine will fire if fuel is present — this establishes whether the problem is ignition or fuel delivery.
- Check that plugs fire (confirmed via bench test).

**Step 3: Logic analyzer on crank/cam/ign/inj**
- Hook up logic analyzer (Sigrok compatible) to: crank signal, cam signal, ignition output (one cylinder), injector output (one cylinder).
- Log both FOME and epicEFI firmware to compare trigger-relative timing.
- This directly shows whether firing order, injection angle, and trigger offset are correct.
- Use the logic analyzer trace to determine: stock idle fuel PW, injector angle, and fuel pump settings from the factory tune if running in limp mode.

**Step 4: Compare firmware behavior**
- The engine ran on FOME firmware but not epicEFI — swap firmwares back and forth to isolate whether the issue is ignition or fuel.
- Suggestion: try epicEFI ignition with factory DME fuel control, to narrow down which subsystem has the problem.
- rusEFI/epicEFI website has a "backup ECU" function that dumps the full flash binary for comparison between firmware versions.

### Reverse Engineering the Factory Binary (Ghidra Workflow)

Context: the team was also attempting to extract injector calibration data from the factory DME binary.

- Tool: **Ghidra** (free NSA decompiler).
- Workflow: alter injector coding in the ECU, read the flash bin again, compare the two binaries to identify which bytes changed — then trace those addresses in Ghidra to understand the function.
- The **INJAD function** was located manually; it contains both multiplicative correction per injector and additive time offset per cylinder.
- Each injector value is stored as **16-bit words (2 bytes)** in different addresses within the function at address **0x800C2D1C** (this is where they are written to RAM at startup).
- Injector coding data was found to be stored in **EEPROM**, not the main flash bin — explaining why it wasn't visible in bin comparisons.
- Cylinders 5–8 have **four separate addresses** for their injector parameters, spaced non-contiguously in memory.
- Suggested enhancement: use Cursor AI (or similar LLM coding tool) to write Python scripts that parse XDF definition files and search for similar patterns in the binary, accelerating the reverse engineering process.

### Plug Condition Notes (End of Session)

Plugs pulled after the session:
- Bank 2 (cylinders 5–8): visibly sooty, not wet — combustion events were occurring but running rich/incomplete.
- Bank 1 (cylinders 1–4): similar condition.
- Conclusion: spark plugs were fouled enough to warrant replacement before the next start attempt; fresh plugs recommended.
