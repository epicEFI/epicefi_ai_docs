# Fuel Cut Code 10 and Injector Duty Cycle Limits

*Source: rusEFI Discord, 2025-11-18 | Channel: 1411925674498719775*
*Contributors: @mykkh*

## Summary

A user running a supercharged engine experienced a sudden fuel cut at 7500 RPM, described as Fuel Cut Code 10. The engine went silent while the supercharger continued to whine with the throttles open. The user also noted that the injector duty cycle channel was absent from their log, raising the question of whether the duty cycle limit had been reached independently of a full fuel cut event.

## Details

### Fuel Cut Code 10 Event

The fuel cut occurred at 7500 RPM on a supercharged engine:

> "A fuel cut lean out at 7500rpm was fun. Suddenly the engine stops making noise and all you hear is the supercharger whine with the throttles open and no exhaust noise. What does Fuel Cut Code 10 mean?" — @mykkh

The specific meaning of Fuel Cut Code 10 in the rusEFI/epicEFI firmware was under investigation. The event presented as a lean-out style fuel cut (engine goes quiet, no exhaust noise) rather than a typical ignition cut.

### Injector Duty Cycle Channel Missing from Log

A separate concern was raised about whether the injector duty cycle limit had been hit without triggering a full fuel cut:

> "I can't find an injector duty cycle channel in the log, I wonder if I hit the duty limit. Although that shouldn't be a full fuel cut." — @mykkh

Key points:
- The injector duty cycle channel was not visible in the log data, making it difficult to confirm whether the duty limit was reached.
- Hitting the injector duty cycle limit is expected to cause injector saturation (fueling falls short) rather than a hard fuel cut.
- A full fuel cut and a duty cycle limit are distinct events with different symptoms; the observed behavior matched a fuel cut more than simple injector saturation.

### Troubleshooting Notes

When investigating similar events:
1. Check which Fuel Cut Code is logged and cross-reference with firmware documentation (Code 10 specifically needs identification).
2. Verify the injector duty cycle channel is enabled in the logging configuration.
3. At high RPM on forced-induction engines, both duty cycle saturation and dedicated fuel cut protections (e.g., RPM-based, AFR-based) can occur close together — distinguish between them using the logged fuel cut code.
