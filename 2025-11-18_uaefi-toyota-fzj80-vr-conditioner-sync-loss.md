# Toyota FZJ80 uaEFI Install: VR Conditioner Selection and Sync Loss with Honda-Style 24+1 Distributor

*Source: rusEFI Discord, 2025-11-18 | Channel: 1435631740969291796*
*Contributors: @El Mecanico 163, @Walter R., @VictorTH, @Walter Ybarra, @Robb235, @Chemex-joshseek*

## Summary

A community member in Venezuela was installing a uaEFI ECU on a 1998 Toyota FZJ80 (automatic transmission) using a Honda-style distributor with a 24-tooth CKP ring and a single-tooth CMP signal (G1/G2 separated by 180 degrees). The engine would start and run initially after timing with a timing light, but after being switched off it would not restart reliably — losing synchronization on the next start attempt. The root cause was the built-in VR conditioner on the uaEFI not properly reading the single-tooth cam signal. The recommended fix was to use an external JDM-style VR conditioner (such as the WTM Tronics JDM VR module) for the single-tooth CMP signal, while keeping the discrete (built-in) conditioner for the 24-tooth CKP. Alternatively, cutting or adding a missing tooth to the 24-tooth wheel eliminates the need for a CMP signal entirely.

## Details

### Vehicle and Trigger Configuration

- **Vehicle**: 1998 Toyota FZJ80 (Land Cruiser), automatic transmission (A440F or similar)
- **ECU**: uaEFI (AlphaX variant)
- **Original system**: Combined PCM controlling both engine and automatic transmission
- **Trigger wheel**: Honda-style distributor with a 24-tooth CKP wheel (similar to Honda B/H/F-series)
- **Cam signal**: Single-tooth CMP, G1 and G2 positions separated by 180 degrees
- **Additional requirement**: IGF (igniter confirmation) signal must be simulated to prevent the automatic transmission from locking into 3rd gear (fail-safe mode)

### Symptom: Engine Starts Once, Then Loses Sync

The engine would start after careful timing setup with a timing lamp. However, once the engine was switched off, it would not restart. Behavior:
- Engine could be cranked endlessly without starting
- Occasionally would start after 6–10 attempts, or not at all until the battery drained
- Changing the trigger edge (rising vs. falling) had no effect
- Adjusting the cylinder 1 offset setting did not resolve the issue — the spark firing of cylinder 1 moved but the offset setting itself had no apparent effect

Walter R. (walterronny) diagnosed this as a likely incorrect trigger angle, and Robb235 (with a similar prior experience on a 5VZ-FE) suspected it matched a known issue with trigger gap override settings or dual ECU VR signal sharing.

### Root Cause: VR Conditioner Inadequacy for Single-Tooth Signal

Multiple community members converged on the same diagnosis: the built-in VR conditioner on the uaEFI does not reliably read the single-tooth CMP signal from the distributor, particularly after the engine stops and the signal conditions change (residual magnetism, tooth position at rest, etc.).

Key observations:
- **Walter Ybarra**: Confirmed this as a known solved problem. He had encountered the same behavior on a uaEFI and resolved it by adding an external VR conditioner. The built-in uaEFI conditioner worked poorly; after replacing it with an external module, the problem disappeared immediately. He had also reproduced this fix for a user in Venezuela.
- **VictorTH**: Noted the pattern — engine starts fine initially but loses sync after shutdown — is characteristic of the single-tooth CMP conditioner failing to re-lock on the next crank cycle.
- **Robb235**: Suspected shared VR sensor grounds between the uaEFI and the original PCM could be causing noise on the VR line (mirror effect / ground loop). He recommended logging the start attempt (.mlg file from TunerStudio) to confirm.

### Recommended Solutions

#### Option 1: External JDM-Style VR Conditioner (Preferred)

Use a dedicated external VR conditioner module on the single-tooth CMP signal:

- **WTM Tronics JDM VR module**: https://wtmtronics.com/product/jdmvr/?v=d4579b2688d6
  - Recommended by VictorTH for single-tooth (1-diente) signals that are difficult to read with standard VR conditioners
  - Walter R.: with this conditioner, both the 24-tooth CKP and 1-tooth CMP can be read reliably without any wheel modification
- **WTM Tronics Mini MAX A2 Signal Conditioner**: https://wtmtronics.com/product/mini-max-a2-signal-conditioner-vr/?v=dd07de856139
  - Also confirmed working by Walter Ybarra

**Wiring approach** (Walter Ybarra):
- Use the JDM VR conditioner for the CMP single-tooth signal (cam input)
- Use the built-in discrete conditioner or separate MAX9924/MAX9926 for the 24-tooth CKP signal

#### Option 2: Modify the 24-Tooth Wheel (No External Conditioner Needed)

Remove (grind off) or add a missing tooth to the 24-tooth CKP ring to create a 24-1 pattern. With a missing tooth, the CKP alone provides full sync information:
- The uaEFI can determine cylinder phase from the missing tooth gap
- No CMP signal required — the single-tooth cam signal can be disconnected
- Walter R.: "if you cut a tooth you can run with just the CKP"
- VictorTH: this is the most viable option if obtaining the external VR conditioner module is difficult
- **Note**: For automatic transmission vehicles, verify that removing the factory CMP signal does not interfere with any remaining OEM TCM functions

#### Option 3: Semi-Sequential / Wasted Spark (Workaround Only)

Configure the ECU for wasted spark ignition and batch/semi-sequential fuel injection, which requires only the 24-tooth CKP and no CMP signal:
- Walter R.: "if you have 24 teeth you cannot run on that alone, you need CMP or cut a tooth"
- This is only a workaround, not a preferred configuration for a 6-cylinder engine with automatic transmission

### Outcome

El Mecanico 163 resolved the problem by quickly adapting a replacement solution (exact method referenced via a photo attachment, not captured in text). He confirmed: "Se acabó el problema" (the problem is over) and "Lo mejor de todo es que no toqué nada de lo original" (the best part is I didn't touch anything on the original system). VictorTH confirmed the engine was revving cleanly, needing only final tune adjustments before street use.

### Diagnostic Tips from This Session

- **Always capture a .mlg log** (TunerStudio: "Start Logging" button) during a failed start attempt. A log when the engine is cranking but not starting gives the most diagnostic information about trigger sync state, RPM reading, and sync counters.
- **Check wiring documentation**: Robb235 emphasized keeping a written record of all wiring connections (which uaEFI pin connects to which vehicle harness wire). Tools like Fritzing or Eagle can be used for schematics. This becomes critical when troubleshooting intermittent sync issues because incorrect pin assignments (e.g., swapped CMP inputs) can produce exactly this symptom.
- **Dual ECU / shared VR sensor concern**: When using rusEFI/uaEFI alongside the original PCM (during testing or partial replacement), the two ECUs may share the same VR sensor signals. Shared grounds between the two units can inject noise onto the VR lines, degrading the signal. Robb235: "I suspect it has to do with two ECUs reading the same VR sensors." Chemex-joshseek: "take out the VR sensors you don't use and cut off their shared grounds — they will make noise in the main VR sensor that you need."
- **Trigger angle**: Walter R. suspected an incorrect trigger angle setting as a contributing factor. Always verify the trigger angle setting matches the engine's TDC tooth position when diagnosing sync loss after initial start.
- **Trigger edge setting**: Robb235 suggested trying to toggle the primary trigger edge (rising vs. falling) when the engine refuses to start after a prior successful start — this can reveal edge-detection issues with VR conditioners that behave differently on the decay vs. rise of the tooth signal.
