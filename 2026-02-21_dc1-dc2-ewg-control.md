# Using DC1 vs DC2 Output for VW Electronic Wastegate Control

*Source: epicEFI Discord, 2026-02-21 | Channel: 1356732771325968630*
*Contributors: @jeybee, @Jackscr*

## Summary

When using the uaEFI board to control a Volkswagen electronic wastegate (EWG), the DC1 output on the bipolar (+/-) output pair is significantly better suited than DC2. DC2 usage on this board is uncommon and its suitability for EWG control is uncertain.

## Details

### DC Output Selection for VW EWG

- `Jackscr` asked whether **DC1** is better than DC2 for driving a VW EWG.
- `jeybee` confirmed: **"DC1 on the +/- output is way better."**
- The recommendation is to use **DC1 on the bipolar (+/-) output**, not DC2.
- DC2 is rarely used on the uaEFI board — the only confirmed use case was driving a stepper motor (by ggurov).

### Notes

- A VW electronic wastegate is a DC motor-driven actuator requiring bidirectional drive (hence the need for the +/- bipolar output capability of DC1).
- DC1 is the preferred channel for any high-current bidirectional DC motor control on this hardware.
