# 24-Tooth VR Wheel Trigger Setup: Sensor Configuration, Skipped Wheel, and Noise Reduction

*Source: rusEFI Discord, 2025-11-18 | Channel: 1435631740969291796*
*Contributors: @Walter_R_, @VictorTH, @Walter_Ybarra, @El_Mecanico_163, @Chemex-joshseek, @Robb235*

## Summary

A multi-user troubleshooting session covering the configuration of a 24-tooth VR crank wheel with a single-tooth cam sensor (JDM-style trigger pattern) in rusEFI/epicEFI. Topics include sensor type selection (discrete vs MAX9924), skipped wheel location placement, noise from unused VR sensors, wasted spark fallback for semi-sequential operation, and handling an automatic transmission that requires IGF signal simulation. A dual-ECU interference problem with shared VR sensors is also identified.

## Details

### Sensor Type Selection: CMP and CKP

For this trigger configuration, the recommended sensor type assignments are:

- **CMP (camshaft position sensor)**: use the **discrete** input setting
- **CKP (crankshaft position sensor)**: use the **MAX9924** (or MAX9926) adaptive threshold VR conditioner setting

> "Tecnicamente debes usar el discrette para el CMP y el Max 9924 para el CKP"
> ("Technically you should use discrete for CMP and Max 9924 for CKP")
> — @Walter_R_

Not all VR sensors are compatible with the system — sensor selection matters for reliable signal conditioning.

> "Que Vr estas usando porque no sirve cualquiera"
> ("What VR are you using because not just any will work")
> — @Walter_R_

### Engine Start but Loses Sync

An engine that starts but then loses synchronization or shuts off does not indicate correct operation. Sync loss after initial start is a failure condition that requires investigation.

> "Puede arrancar pero si pierde sincronizacion o se apaga no significa que funcione"
> ("It can start but if it loses synchronization or shuts off it doesn't mean it works")
> — @Walter_R_

### Skipped Wheel Location Setting

When reading 24 teeth from the camshaft sensor, the skipped wheel location must be placed in the **cam** settings, not the crank settings:

> "ahora como estas leyendo los 24 dientes en el arbol de levas coloca skipped wheel location en cam"
> ("now as you are reading the 24 teeth on the camshaft, place skipped wheel location in cam")
> — @VictorTH

### UAEFI VR Reading Issue

A known issue was identified where the UAEFI hardware has a problem with VR signal reading:

> "veo q la UAEFI tiene un problema con los VR"
> ("I see that the UAEFI has a problem with VR sensors")
> — @Walter_Ybarra

### Semi-Sequential vs Wasted Spark

When running in semi-sequential injection/ignition mode with a VR trigger that the system cannot resolve for full sequential operation, the recommendation is to reconfigure to **wasted spark** mode:

> "osea lo que puedo entender es que con este vr no lo voy a lograr" / "pues si esta en semi secuencial si"
> ("so what I understand is that with this VR I won't achieve it" / "well yes if it's in semi-sequential mode")
> — @El_Mecanico_163, @Walter_Ybarra

> "configuro entonces wasted spark?"
> ("configure wasted spark then?")
> — @El_Mecanico_163

### Cutting One Tooth: 24-1 Wheel

The most reliable solution for solving ignition sync issues with a 24-tooth wheel is to physically cut (remove) one tooth, converting it to a 24-1 pattern. This gives the ECU an unambiguous sync reference:

> "la forma mas viable actualmente, es que cortes un diente de los 24 y ahi se acabo el problema de no encender"
> ("the most viable approach currently is to cut one tooth from the 24 and that solves the no-start problem")
> — @VictorTH

> "si cortas un diente puedes usar solo el CKP."
> ("if you cut one tooth you can use only the CKP")
> — @Walter_R_

With a 23-tooth (24-1) pattern on the crank, the cam sensor becomes optional for engine management.

### JDM Single-Tooth Cam Configuration

An alternative to cutting a tooth is to use the **JDM** trigger configuration with a single-tooth cam signal:

> "puedes hacer esta combinacion usas la JDM para un diente la señal CAM"
> ("you can use this combination: use JDM for one tooth for the CAM signal")
> — @Walter_Ybarra

> "y el CRANK usas el discreto"
> ("and for the CRANK use the discrete input")
> — @Walter_Ybarra

> "con el VR JDM puedes usar los 24 y el 1 diente"
> ("with the VR JDM you can use the 24 teeth and the 1 tooth")
> — @Walter_R_

Pin assignments suggested for the VR JDM configuration: **pins 24 and 1**.

### Automatic Transmission Considerations

For an automatic transmission vehicle where a single PCM controls both engine and transmission:

- The transmission requires **TPS** and **speed sensors** to be properly connected
- If replacing or bypassing the OEM ECU with rusEFI/epicEFI, the new ECU must provide TPS and VSS signals that the transmission logic expects

> "lo que necesitas para la transmission es la señal tps y que tenga los sensores de velocidad conectados"
> ("what you need for the transmission is the TPS signal and speed sensors connected")
> — @VictorTH

> "si si es un pcm funciona para motor y caja"
> ("yes it's a PCM that handles both engine and gearbox")
> — @El_Mecanico_163

### IGF Signal Simulation for Gearbox

On vehicles where the factory PCM uses an IGF (Ignition Confirmation / Igniter Feedback) signal to control the automatic transmission, the absence of this signal causes the gearbox to default to third gear (limp mode). When replacing the OEM ECU, the IGF signal must be simulated:

> "includo debo simular la señal igf para que la caja no quede en 3era marcha"
> ("I also need to simulate the IGF signal so the gearbox doesn't stay stuck in third gear")
> — @El_Mecanico_163

### Removing Unused VR Sensors to Reduce Noise

If the engine has multiple VR sensors wired (e.g., from a different OEM trigger setup) but only one is needed, the unused sensors should be physically removed or their shared grounds cut. Unused VR sensors share grounds with the active sensor and inject noise into its signal:

> "take out the vr sensors u dont use and cut off their shared grounds they will make noise in the main vr sensor that u need"
> — @Chemex-joshseek

> "I will only use VR for the 24 teeth"
> — @El_Mecanico_163

### Trigger Gap / Space Cancellation for Parallel Configuration

For engines configured in parallel mode (e.g., Toyota 5VZ-FE), issues can be resolved by adjusting the **trigger space cancellation** (trigger gap) settings:

> "Porque tuve un problema similar con la configuración en paralelo de mi 5vz-fe. Tuve que ajustar la configuración de anulación del espacio del gatillo y el problema se solucionó."
> ("I had a similar problem with the parallel configuration of my 5VZ-FE. I had to adjust the trigger space cancellation setting and the problem was solved.")
> — @Robb235

### Post-Shutdown No-Restart Issue

A reported symptom where the engine starts and syncs correctly with a timing light but cannot be restarted after shutdown. This is a known intermittent issue that should be logged for analysis:

> "No, you turn it on, synchronize it with the timer lamp, leave everything in order, but when you turn it off it doesn't turn back on."
> — @El_Mecanico_163

To diagnose, record a startup attempt log and save it as an **.mlv file** using the rusEFI console. The full software license is not required to record logs:

> "No necesitas comprar la versión completa. Aún puedes grabar un registro de los intentos de arranque del motor y enviarme el archivo .mlv"
> ("You don't need to buy the full version. You can still record a log of engine start attempts and send the .mlv file")
> — @Robb235

### IAT Sensor Pull-Up Resistor

For the intake air temperature (IAT) sensor, if the ECU already provides an internal pull-up resistor on the analog input, an external pull-up resistor should be removed to avoid double-biasing the thermistor divider:

> "for the IAT I removed the pull-up resistor"
> — @El_Mecanico_163

### Dual ECU Interference on Shared VR Sensors

In setups where two ECUs are both wired to the same VR crank/cam sensors simultaneously, interference and misreading are expected. Each ECU's input stage loads the sensor differently and the signal conditioners can conflict:

> "Sospecho que tiene que ver con que dos ECUs leen los mismos sensores VR"
> ("I suspect it has to do with two ECUs reading the same VR sensors")
> — @Robb235

If running a dual-ECU setup, isolating or buffering the VR sensor signals between ECUs is necessary for reliable operation.
