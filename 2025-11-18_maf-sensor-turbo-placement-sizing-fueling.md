# MAF Sensor Placement, Sizing, and Fueling in Turbocharged Engines

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat-2*
*Contributors: @ToyotaTerrorist, @Robb235, @Neos6443, @ggurov*

## Summary

An extended community discussion covering the practical considerations for using a Mass Air Flow (MAF) sensor in a turbocharged engine: where to place it relative to the turbocharger, how to size it for the expected airflow, how to compare MAF vs speed-density (SD) air mass calculations in the data logger, and how rusEFI's mafmap feature allows blending MAF and MAP fueling sources per RPM and load region. The group also covered flutter mitigation when using MAF near the throttle body and the option to fall back to SD below vacuum.

## Details

### Pre-Turbo vs Post-Turbo MAF Placement

Community members debated whether a MAF sensor should be installed upstream (draw-through / pre-turbo) or downstream (blow-through / post-turbo) of the turbocharger.

- **@ToyotaTerrorist**: Was open to a pre-turbo installation but skeptical about relying on MAF for primary fuel control — "Im more than happy to hook one up pre turbo? im just very sceptical on actually letting it control fueling."
- **@Robb235**: Recommended post-turbo placement initially as a data-collection exercise rather than for active fuel control: "I would hook it up post turbo. Just use it to collect data points. You can pull it up in MLV and see SD air mass vs MAF air mass calcs."
- **@Neos6443**: Noted that some MAF sensors require pre-turbo placement due to their design: "Some MAFs are fussy and have to be pre-turbo. Some MAFs are fine with a blow-through setup."
- **@Robb235** on the physics: "Air mass is air mass, whether the air is compressed or not." The pre-turbo draw-through setup tends to be more accurate in steady state, but has a known weakness.
- **@Neos6443** on the blow-off valve exception: "Correct, but draw-through tends to be more accurate (except for when a blow-off valve opens, which is when you switch to MAP)."

### Off-Throttle Flutter and MAF Position Relative to Throttle Body

- **@Robb235**: "If MAF is close to the tb seems like off throttle flutter would be a non issue. Where as if MAF has a bunch of charge pipe between it and the tb, then I can see air back flowing across the MAF when throttle snaps shuts."
- **@ggurov**: When using the mafmap blending feature, flutter below vacuum is a non-issue because the system switches back to SD mode: "you're not running map below vac, you switch back to SD, so flutter doesn't matter."

### rusEFI mafmap — Blending MAF and MAP Fueling Sources

rusEFI supports a `mafmap` (MAF + MAP blend) configuration that allows operators to specify, per RPM x MAP cell, how much of the fueling comes from the MAF sensor vs speed-density.

- **@ggurov**: "yes, we have mafmap, you set rpm x map where, and how much maf fueling source you want."
- **@ToyotaTerrorist** acknowledged this as a workaround for covering the full operating range: "Yeah thats definitely a work around."
- The system falls back to SD below vacuum so that off-throttle backflow across the MAF does not corrupt fueling.

### MAF Sensor Sizing and Flow Rate Limits

- **Rule of thumb (@Robb235)**: "General rule of thumb of 1 lbs/min per 10hp." Use this to estimate peak required MAF flow from peak power figures.
- To determine peak flow: ask "What is peak hp?" then calculate peak air mass (lbs/min) = peak HP / 10.
- **Toyota MAF in 2.5" tube (@Robb235)**: "Toyota MAF is good up to 30 lbs/min in 2.5" diameter tube. I have it scaled up to 3" diameter, hang on I'll go look what that max is."
- **1.8T MAF sensor (@ToyotaTerrorist)**: "Apparently you can run the 1.8t sensor accurately upto 35lb an hour in the stock housing. Upgrading to the 3 inch should net close to 45."

### Comparing MAF and SD in the Data Logger

- @Robb235 recommended logging both MAF-calculated air mass and SD-calculated air mass simultaneously in MegaLogViewer (MLV) before committing to MAF-based fueling. This allows calibration and validation of the MAF transfer function against the known SD model.

### MAF Fueling User Experience

- @Robb235 solicited real-world feedback on MAF fueling performance (transition speed, overall accuracy) from other users. No detailed response was captured in this batch; the question remained open.
- @ToyotaTerrorist expressed concern about repeated engine damage from MAF misconfiguration: "I just dont wanna blow up engines again goes to maf."
