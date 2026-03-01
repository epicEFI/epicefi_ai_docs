# Per-Cylinder Fuel Trim in rusEFI/epicEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1356732771325968630*
*Contributors: @Joesphan, @Alice*

## Summary

rusEFI/epicEFI supports per-cylinder fuel trim, allowing individual fuel injection offsets for each cylinder. This feature can be used to compensate for injector-to-injector flow variation by testing and marking each injector individually.

## Details

### Per-Cylinder Fuel Offset Feature

rusEFI/epicEFI provides the ability to offset each cylinder's fuel injection independently — either adding or reducing the amount of fuel delivered to that cylinder. This is referred to as "per cylinder trim."

### Use Case: Injector Flow Matching

One practical application is compensating for injector-to-injector variation. The workflow discussed was:

1. Test each injector individually to measure its actual flow rate
2. Mark/label each injector with its test results
3. Apply a per-cylinder trim offset in the ECU configuration to normalize fueling across all cylinders

This allows the ECU to account for slight differences between injectors without needing to replace or flow-match them externally.
