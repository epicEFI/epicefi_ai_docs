# EMTRON-Style Speed-Density: RPM vs TPS VE Table with EMAP/IMAP Compensation

*Source: rusEFI Discord, 2025-11-18 | Channel: 1389299516808630312*
*Contributors: @ggurov*

## Summary

A discussion of an alternative speed-density tuning strategy inspired by EMTRON ECUs, where the main VE (volumetric efficiency) table uses RPM vs TPS as its axes instead of the traditional RPM vs MAP. Exhaust manifold pressure (EMAP) and intake manifold absolute pressure (IMAP) compensation are then layered on top, either as a simulated value or a real sensor input. This approach reduces the number of cells that need to be individually tuned because the ECU interpolates between breakpoints, and the goal is to stay within 5% accuracy with linear interpolation active.

## Details

### Motivation: Reducing Map Size

Standard VE tables can be large, requiring many individually-tuned cells. The objective described here is to reduce the table size while keeping accuracy within 5% by relying on linear interpolation between a smaller set of calibrated points.

> "the ask was to reduce the map and be within 5% when linear interpolation is active"
> — @ggurov

> "unless we reduce the main table sizes"
> — @ggurov

> "fewer cells to tune, ECU does interpolation"
> — @ggurov

### Speed-Density Revelations

Work on tuning strategy led to new insights about speed-density approaches:

> "there's also been some... revelations as far as speed density tuning"
> — @ggurov

### EMTRON-Style Architecture

The EMTRON approach uses a different axis set for the primary VE table:

- **Main VE table axes**: RPM vs TPS (throttle position) rather than RPM vs MAP
- **Compensation layer**: EMAP (exhaust manifold absolute pressure) and IMAP (intake manifold absolute pressure) corrections applied on top of the base VE value

> "EMTRON-style speed-density"
> — @ggurov

> "where it's RPM vs TPS for main VE table reference"
> — @ggurov

> "and either a simulated, or real EMAP/IMAP compensation up top"
> — @ggurov

### RPM vs TPS vs RPM vs MAP

Using TPS as the load axis instead of MAP provides a more direct measurement of operator demand and can be more stable in certain engine configurations (e.g., naturally aspirated engines with large throttle bodies). The EMAP/IMAP compensation then corrects for actual manifold pressure conditions, allowing the base table to remain simpler.

Either a real EMAP sensor can be wired and configured, or a simulated EMAP value can be used in cases where hardware is not available.
