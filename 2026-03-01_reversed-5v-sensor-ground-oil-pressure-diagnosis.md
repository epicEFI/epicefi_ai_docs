# Reversed 5V and Sensor Ground Wiring — Oil Pressure Sensor Not Reading

*Source: rusEFI Discord, 2026-03-01 | Channel: 1356732771325968630*
*Contributors: @Bearded_Bogan, @Joesphan*

## Summary

A sensor harness carrying IAT, oil pressure, MAP and other analog sensors was wired with the 5V supply and sensor ground reversed at the crimp connection to the external harness. This caused the oil pressure sensor to read nothing in the ECU while all other sensors on the same supply (CLT, IAT, TPS, PPS) continued to function. Once the polarity was corrected the oil pressure reading was restored, though a separate cam-sync issue remained.

## Details

The troubleshooting session began with the Bosch oil pressure sensor returning zero output at the ECU despite the engine clearly having oil pressure (confirmed by oil spilling from the port when the sensor was removed and by healthy cranking sound). All other 5V-referenced sensors — CLT, IAT, TPS, PPS — worked correctly, which pointed away from a shared 5V supply fault.

Key diagnostic steps taken before the root cause was found:

1. Continuity check between 5V and sensor ground with the ECU and all 5V sensors disconnected confirmed no wiring short (open circuit as expected).
2. Bench test of the Honeywell sensor was discussed (power it up, apply pressure via a tube, measure voltage change with a multimeter) but could not be completed due to a faulty DMM.
3. A spare sensor was swapped in — this led to the discovery described below.

Root cause: during crimping of the external sensor harness, the 5V supply wire and sensor ground wire were transposed. The reversal affected only the oil pressure sensor reading at the ECU; the other sensors on the same harness still functioned (likely because they present a higher input impedance or have internal protection). Correcting the crimp polarity restored oil pressure readings.

The same session also revealed a separate cam-sensor issue (no cam sync signal in the trigger log), which was not caused by the wiring reversal and required further investigation of ECU pin assignments and sensor types.

## Notes

- A working multimeter is essential for this kind of diagnosis — measuring voltage at the sensor output (0.5 V at vacuum, scaling to 4.5 V at full pressure for a typical MAP/pressure sensor) would have identified the reversal immediately.
- The reversal did not blow any protection — after correcting polarity, hardware was confirmed functional. However, sustained reverse-polarity on a sensor can damage the sensor or ECU input in some designs; inspect carefully before re-powering after a confirmed reversal.
- The cam-sync fault (separate issue) showed no cam signal in the trigger log even after correcting the harness polarity, suggesting a different root cause (wrong pin assignment or wrong sensor type for that input).
