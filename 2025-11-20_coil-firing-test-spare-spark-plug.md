# Coil Firing Verification Test Using a Spare Spark Plug

*Source: rusEFI Discord, 2025-11-20 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov)*

## Summary

Simple field diagnostic method for confirming ignition coil output using a spare spark plug, useful when an engine cranks but does not start and ignition is suspected.

## Details

**Procedure:**
1. Bring an **extra/spare spark plug** to the vehicle.
2. **Pull one coil** from its plug well (disconnect the coil-on-plug unit from the cylinder).
3. **Attach the spare plug** to the coil's output (insert the plug into the coil boot).
4. Ground the spare plug's body to the engine block or chassis (use a jumper wire or physically hold it against bare metal — do not hold it by hand near the electrode).
5. Crank the engine while observing the spare plug for a spark.

**Interpretation:**
- If the plug **fires visibly**, the coil, its wiring, and the ECU ignition output for that cylinder are functional.
- If the plug **does not fire**, the fault is in the coil, its connector/wiring, the ECU ignition driver, or the trigger/sync signal not reaching that cylinder's firing event.

**Notes:**
- This test bypasses compression resistance — a coil that passes this test may still struggle to fire under high compression. Use a proper in-cylinder test for definitive diagnosis.
- Repeat on all coils if a misfire is suspected across multiple cylinders.
- Applicable to any coil-on-plug or coil-near-plug setup managed by rusEFI/epicEFI.
