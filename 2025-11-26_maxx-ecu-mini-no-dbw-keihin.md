# Maxx ECU Mini Does Not Support Drive-By-Wire Throttle

*Source: rusEFI Discord, 2025-11-26 | Channel: 1411925674498719775*
*Contributors: GGGI (@.gunadi), SanGawku (@sangawku)*

## Summary

The Maxx ECU Mini does not support drive-by-wire (DBW / electronic throttle control) systems. For Honda applications with a Keihin DBW throttle body, the stock ECU with Honda Tuning Suite (HTS) was suggested as an alternative, though that was not acceptable for a dedicated drag race build.

## Details

**Platform limitation:**
The Maxx ECU Mini cannot drive or control a drive-by-wire throttle body. Customers expecting DBW support need to select a different ECU.

**Application context:**
The throttle body in question was a **Keihin** unit (Honda application). The user had already decided against the Maxx ECU Mini for this reason.

**Suggested alternative:**
SanGawku suggested using the **stock Honda ECU with Honda Tuning Suite**, which likely supports the factory Keihin DBW throttle. However, this was not suitable for the intended use case (drag racing), as the customer wanted to replace the stock ECU entirely.

**Takeaway for rusEFI/epicEFI users:**
This thread arose in an epicEFI support context. epicEFI/rusEFI hardware (E840 and similar) does support DBW throttle control, which may make it a suitable alternative where the Maxx ECU Mini falls short in DBW applications.
