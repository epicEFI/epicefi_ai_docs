# CAN Bus Feature Requests and Community-Built Accessories

*Source: epicEFI Discord, 2026-02-26 | Channel: 1411925674498719775*
*Contributors: @TacoBellFool, @Joesphan*

## Summary

Community members discussed desired hardware features for future rusEFI/epicEFI ECU revisions, particularly dual CAN bus support and a faster PID loop. Separately, the community noted a growing trend of members building their own CAN bus peripheral devices — a sign of the platform's expanding ecosystem and user skill base.

## Details

### Feature Requests for Future ECU Hardware

- **Dual CAN bus:** Having two independent CAN bus interfaces would allow simultaneous connection to both a vehicle CAN network (e.g., OEM ABS, TCU, instrument cluster) and a secondary peripherals bus (e.g., PDM, dash, traction control nodes) without a CAN bridge or gateway device.
- **Faster PID loop:** A higher-frequency PID execution rate was requested to improve closed-loop control performance (ETB, boost control, idle, etc.). The current loop speed was felt to be a limiting factor in control precision.

These were expressed as wishlist items for future hardware revisions, not as currently supported features.

### Community CAN Bus Accessories

A notable trend in the rusEFI community is members designing and building their own CAN bus peripheral devices — items like custom dash controllers, PDMs, and sensor nodes that communicate with the ECU over CAN. This represents a shift beyond traditional tuning (fuel/ignition maps) toward integrated systems engineering.

> "We got guys on here making their own canbus accessories, people that were only wrench monkeys."

This reflects the platform's open-source nature and the modular, CAN-extensible architecture of rusEFI/epicEFI, which lowers the barrier for community hardware development.
