# Proteus ECU Output Capabilities

*Source: epicEFI Discord, 2026-02-28 | Channel: 1411925674498719775*
*Contributors: @SanGawku, @ggurov, @GGGI*

## Summary

The rusEFI Proteus ECU provides 16 low-side drivers, 12 ignition/general-purpose outputs, 4 high-side outputs, and dual H-bridges for electronic throttle control. The ignition outputs can also be repurposed as gate drivers for external high-side MOSFETs when additional high-side switching capacity is needed.

## Details

The Proteus ECU output complement, confirmed from the board specification:

| Output Type | Count | Rating |
|---|---|---|
| Low-side drivers | 16 | 4A per channel |
| Ignition / general-purpose outputs | 12 | 5V gate drive |
| High-side outputs | 4 | 12V, 3A per channel |
| H-bridge pairs (ETB) | 2 (dual) | For electronic throttle body |

### Low-Side Drivers

The 16 low-side drivers are the primary switching outputs for injectors, solenoids, relays (fuel pump, main relay, cooling fan), and any other load that is switched to ground. Each channel is rated at 4A continuous.

### Ignition Outputs

The 12 ignition outputs are 5V gate-drive signals intended for IGBT-based ignition coils. Because they are gate drivers rather than power outputs, they can also be used as general-purpose 5V logic outputs. @ggurov noted that these can drive the gates of external high-side MOSFETs, effectively extending the number of high-side switching channels beyond the four dedicated high-side drivers — useful for TCU-style applications that require more high-side outputs than the board provides natively.

### High-Side Outputs

Four dedicated 12V high-side outputs at 3A each are available for loads that require a switched 12V supply rather than a ground switch. The community noted that four may be limiting for transmission control unit (TCU) applications, where multiple high-side channels are common.

### Electronic Throttle Control

The dual H-bridges support direct connection of an electronic throttle body (ETB) motor. The H-bridge implementation also supports a stepper motor idle air control valve as an alternative use.

## Notes

- Output count was confirmed by looking up the Proteus board specification during the conversation; @ggurov initially estimated 16 low-sides and 4 high-sides from memory, which GGGI's full spec breakdown confirmed.
- For TCU builds needing more high-side channels, using ignition outputs as gate drivers for discrete MOSFETs is a documented workaround.
