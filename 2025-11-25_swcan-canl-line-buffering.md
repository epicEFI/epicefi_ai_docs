# SWCAN / CAN Bus CANL Line Handling: Buffering vs. Jumper to Ground

*Source: rusEFI Discord, 2025-11-25 | Channel: 1430309988688990298*
*Contributors: SanGawku (@sangawku), Joesphan (@joesphan)*

## Summary

When interfacing a Single-Wire CAN (SWCAN) transceiver alongside a standard two-wire CAN bus, directly jumpering CANL to ground is not recommended. Buffering the CANL line is the preferred approach to prevent the SWCAN transceiver's ground-referenced operation from disrupting the differential CAN bus and to prevent the standard CAN bus traffic from interfering with the SWCAN segment.

## Details

**Background:**
The discussion arose in the context of deciding whether to run two separate physical CAN buses (one standard dual-wire CAN, one dedicated SWCAN segment with its own transceiver) or to simplify by using a jumper to loop CANL directly to ground on the standard CAN bus side.

**Why not jumper CANL to ground:**
On a standard CAN bus, CANL is a differential line that swings between approximately 1.5 V (recessive) and 1.0 V (dominant). Tying it hard to ground collapses the differential pair and will disrupt any other nodes on the same CAN segment from communicating. It also creates a path for ground offsets to inject noise onto the bus.

**Recommended approach — buffer the line:**
> "You could buffer the line" — Joesphan

A line buffer (e.g., a CAN-capable bus buffer IC or a dedicated SWCAN transceiver with proper isolation) should be used to interface the SWCAN segment to the rest of the network. Joesphan also noted that without proper isolation, other nodes on the bus ("the dash") may not correctly receive or may be disturbed by traffic intended for the SWCAN segment:

> "Not sure if the dash will listen to the other people yapping" — Joesphan

**Practical recommendation:**
- Use a dedicated SWCAN transceiver IC (e.g., TH8056 or NCV7356) on the SWCAN segment rather than adapting a standard dual-wire CAN transceiver.
- If the application requires both standard CAN and SWCAN, run them as electrically separate buses with a gateway or bridge, rather than attempting to tie CANL to ground as a workaround.
