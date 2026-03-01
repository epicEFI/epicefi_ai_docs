# epicTuner Java Version Compatibility: Bridging Java 11 to Java 8
*Source: rusEFI Discord, 2025-11-19 | Channel: 1440539911059931259*
*Contributors: @MarkCraftPerformance (carolinacumberland)*

## Summary

epicTuner compiles to Java 11 bytecode, but TunerStudio (TS) expects Java 8 bytecode. A build-time bridge step is required to downcompile Java 11 bytecode to Java 8 bytecode before epicTuner can be used as a TunerStudio plugin.

## Details

### Problem

- **epicTuner** targets **Java 11** (or is compiled with Java 11 language features / class file version).
- **TunerStudio (TS)** expects plugins/components compiled against **Java 8** bytecode.
- Directly loading a Java 11-compiled jar into TunerStudio results in a version incompatibility error.

### Solution

A bytecode downconversion bridge step was created:
1. Build epicTuner normally (produces Java 11 bytecode).
2. Run a conversion step (e.g., using a tool such as `retrolambda` or similar bytecode transformer) to convert the Java 11 `.class` files / jar down to Java 8 compatible bytecode.
3. The resulting Java-8-compatible artifact can then be loaded by TunerStudio.

### Notes

- This is a build/packaging concern, not a runtime issue. End users receiving a pre-built distribution should not encounter this if the build pipeline already applies the conversion.
- Developers building epicTuner from source need to include this bridge step in their build process to produce a TS-compatible output.
- The Java version mismatch is a known integration constraint between modern JVM toolchains and TunerStudio's older runtime requirements.
