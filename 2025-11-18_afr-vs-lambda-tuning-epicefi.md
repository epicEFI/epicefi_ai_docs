# AFR vs Lambda in Engine Tuning with epicEFI / rusEFI

*Source: rusEFI Discord, 2025-11-18 | Channel: 1392172039430738111*
*Contributors: @ggurov, @DCwerx, @MykkH, @Robb235, @Mr_Zebra, @Joesphan, @CarolinaCumberland*

## Summary

A detailed community discussion covering the relationship between Air-Fuel Ratio (AFR) and Lambda values in the context of rusEFI/epicEFI engine management. Key topics include: the correct stoichiometric mapping (14.7 AFR = Lambda 1.0 for all gasoline blends including ethanol mixes), common misunderstandings about gauge readings, how epicEFI stores and displays these values, practical idle AFR targets, the importance of the "world fuel correction" setting, and how to change the sensor type from O2 to Lambda in the project properties.

## Details

### AFR and Lambda Fundamentals

The stoichiometric air-fuel ratio for gasoline is 14.7:1, which corresponds exactly to Lambda 1.0. This relationship holds true regardless of ethanol content when using the gasoline AFR scale:

> "the afr table is a lie, 14.7 is actually lambda 1, any e content"
> — @ggurov

> "even if you are at e80, if your target table is in afr, 14.7 there is lambda 1 to ecu"
> — @ggurov

> "I do this. I run e85 and I keep everything on the gasoline scale where 14.7 is stoich is lambda 1.0"
> — @Robb235

Lambda is preferred for active tuning because error calculations are simpler — a deviation from Lambda 1.0 has a consistent meaning regardless of fuel type.

### Why Lambda is Recommended for Tuning

> "This is why it's recommended to use lambda. I'm old and think on the AFR scale and I'm totally content using 14.7 as lambda 1."
> — @MykkH

> "Funny I set up tables in afr because that's what I learnt. While tuning and logging I switch to lambda because it's easier to calculate errors"
> — @DCwerx

Using Lambda during datalogging simplifies identifying rich/lean excursions since Lambda 1.0 is always stoichiometric, regardless of fuel blend.

### Misinterpretation of Gauge Readings

A common mistake is treating the AFR number displayed on a gauge as a direct measurement rather than a lambda-derived conversion:

> "I think the problem occurs when people don't realize AFR on the gauge is a conversion from lambda so they start targeting richer than necessary thinking it's a direct representation"
> — @MykkH

> "I've had people look at my AEM AFR gauge, knowing that I run E85, and trying to tell me I'm running too lean cause I'm cruising at 14.7"
> — @Robb235

The AFR value on a wideband gauge is always a lambda-derived conversion using a stoichiometric reference. For E85, the true stoichiometric AFR is ~9.8:1, but on the gasoline scale the gauge would show 14.7 at Lambda 1.0.

### Lambda and AFR Tables in epicEFI

The lambda table and AFR table in the tune file are not two separate datasets — they reference the same underlying values:

> "the lambda table and afr table share the same data, it's the same values stored in the tune, but display on ts side is changed between afr gasoline, and lambda"
> — @ggurov

This means switching the display between AFR and Lambda in TunerStudio is purely cosmetic; the stored target value does not change.

EpicEFI allows displaying both Lambda and AFR gauges simultaneously:

> "you can pull up gauges both lambda and afr with epic"
> — @ggurov

This is similar to a Speeduino feature and contrasts with Megasquirt, which does not offer lambda gauge display when the project is configured in AFR mode:

> "One thing that speeduino has that is awesome and megasquirt doesn't is TS for lambda. Even if the project is set up for AFR you can pull up gauges based on lambda"
> — @DCwerx

### Changing Sensor Type to Lambda in Project Properties

Users can change the sensor type from O2 to Lambda directly in the epicEFI project properties:

> "I may be wrong but we can't change it in epic project properties? ... Disregard we can change to lambda"
> — @DCwerx

### What the O2/Wideband Sensor Actually Measures

> "it measures oxygen"
> — @ggurov

The wideband oxygen sensor measures residual oxygen concentration in the exhaust, from which the lambda value is derived.

### Idle AFR Targets

A voltage/fuel reading of 13.2 AFR was noted as acceptable for initial bring-up on E10 fuel:

> "but for now, 13.2 is okay, do mind the fuel you are using is probably e10"
> — @ggurov

For idle specifically, richening slightly from stoichiometric is often beneficial:

> "You can richen up idle to like 14.0. Not all cars will want to idle well at 14.7"
> — @DCwerx

A user asked whether 13.7–13.9 AFR was suitable for idle:

> "Yeah I can't stand it, would 13.7-13.9 be ok for idle or should it be leaner"
> — @CarolinaCumberland

The consensus is that idle AFR between ~13.5 and 14.5 is typical, and the exact target depends on the specific engine's idle behavior.

### World Fuel Correction and First Start

The "world fuel correction" setting in epicEFI is critical for correct fueling, especially with mixed or ethanol-blended fuels. Getting this wrong can prevent the engine from idling even if it starts:

> "My first start on epic it would hit but not idle. GG told me to change the world fuel correction and it idled. So very important to know what is going in your tank with epic"
> — @CarolinaCumberland

### Lean-Out Cruise After Initial Tune

Once a base tune is stable, fuel economy can be improved by leaning out the cruise cells:

> "once you're running better, you'll lean out cruise, and all the are under where boost starts to make a difference"
> — @ggurov

This means the cruise AFR/lambda adjustments should focus on the low-load cells below the boost threshold.
