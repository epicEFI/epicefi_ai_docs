# CAN Bus: BMW F10 8HP70 Reverse Engineering, epicEFI Throughput, and CAN FD

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: fast335xi (@fast335xi), ggurov (@ggurov), Tera (@teraflopping), Joesphan (@joesphan)*

## Summary

Discussion covering BMW F10 8HP70 transmission CAN bus reverse-engineering resources, epicEFI CAN bus throughput benchmarks, CAN FD bandwidth, BMW F-series architecture (CAN-only, no FlexRay on the 8HP), and the status of an in-progress 8HP70 CAN integration in epicEFI.

## Details

**BMW F10 8HP70 CAN bus reverse-engineering resource:**

fast335xi shared a detailed blog post covering CAN bus reverse engineering for the BMW F10 with 8HP70 transmission:
https://spoolas.eu/blogs/news/bmw-f10-8hp70-can-bus-reverse-engineering

This is a useful reference for anyone implementing ZF 8HP transmission control via rusEFI/epicEFI on an F-series BMW.

**BMW F-series 8HP architecture — CAN only, no FlexRay:**

A key finding confirmed in this discussion: the **BMW F-series 8HP70 transmission communicates over standard CAN, not FlexRay**. FlexRay is present in the F-series chassis but is used for DSC (Dynamic Stability Control) and similar chassis systems — it gets converted to CAN at a gateway. The 8HP itself is accessible via CAN only.

This is significant because FlexRay integration with a standalone ECU is substantially harder than CAN. The F-series 8HP is therefore a more tractable target for epicEFI transmission control than initially feared.

**In-progress epicEFI 8HP70 CAN integration:**

fast335xi reported having all CAN message IDs defined and all code/mode mappings completed. The remaining work at the time of this conversation was coding the actual implementation and adding CRC/checksum handling for the 8HP CAN messages (ZF 8HP uses checksums in its CAN frames).

**Torque request message for transmission control:**

Tera noted that the 8HP needs a torque request message sent over CAN from the ECU to operate normally. This is the primary interface required from the engine management side — the transmission expects to receive torque information from the engine ECU to make shift decisions and slip control calculations.

**epicEFI CAN throughput benchmark:**

ggurov reported that epicEFI has achieved a peak CAN bus message rate of **3,200 packets per second**. A separate channel discussion link was also referenced (Discord internal link to channel 1431687969239994470).

Additional epicEFI CAN/comms bandwidth figures mentioned:
- CAN bus: up to **1 Mbps** supported on epicEFI
- USB comms: epicEFI idles at **6 Mbps** on the USB bus

**CAN FD bandwidth:**

Tera noted that CAN FD (Controller Area Network with Flexible Data-rate) supports up to approximately **2 Mbps** in the data phase. Standard CAN is limited to 1 Mbps. CAN FD was noted as a potential path if higher throughput is needed, though the conversation framed it as future consideration — standard CAN at 1 Mbps is sufficient for current 8HP70 integration work.

**FlexRay context (for reference):**

FlexRay operates at up to 10 Mbps and is deterministic (time-triggered). It is used in some BMW F-series chassis systems. Dedicated FlexRay ICs exist for interfacing with it, but there is no current plan in rusEFI/epicEFI to implement FlexRay support. The consensus was to work with CAN-accessible systems and revisit FlexRay only if no CAN alternative exists.
