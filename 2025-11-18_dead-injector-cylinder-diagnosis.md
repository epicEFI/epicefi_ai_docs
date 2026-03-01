# Diagnosing Dead Injectors and Dead Cylinders by Sequential Disable

*Source: rusEFI Discord, 2025-11-18 | Channel: general-chat-2*
*Contributors: @ggurov, @ZeeKay*

## Summary

When an engine runs poorly and a dead injector or dead cylinder is suspected, a simple diagnostic method is to disable (or physically unplug) injectors one at a time while the engine is running and listen for a change in engine behavior. rusEFI supports sequentially disabling individual injectors from the software side. An alternative hands-on approach is to pull injector connector plugs one by one down the bank. The cylinder that shows no RPM drop or sound change when its injector is disabled is the dead or non-contributing cylinder.

## Details

### Software Method — Sequential Injector Disable in rusEFI

- **@ggurov**: Described using rusEFI to sequentially turn injectors off one by one on a six-cylinder engine: "sequential 6 turning injectors off one by one."
- Purpose: "to tell by sound if one injector is dead" and to confirm "or one cylinder is dead."
- When you disable the injector for a healthy cylinder, the engine will stumble, RPM will drop, or the exhaust note will change noticeably. If disabling an injector produces no change, that cylinder is either already not contributing (dead injector, dead cylinder, compression issue).

### Physical Method — Pull Injector Plugs

- **@ZeeKay**: "yeah I usually just pull the injector plug down the row to see which one isn't working lol."
- This is the simplest field method: physically unplug each injector harness connector in turn while the engine idles. The cylinder that causes no change when unplugged is the suspect.

### Listening for Injector Operation

- A healthy injector produces an audible clicking or buzzing sound from the solenoid during normal operation.
- Absence of this sound when the engine is running indicates the injector is not receiving a pulse, has a failed solenoid, or has an open circuit in the wiring.
- Combining the sound check with the sequential-disable test narrows the fault to either the injector itself or the cylinder's mechanical condition.
