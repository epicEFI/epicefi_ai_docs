# GDI8 Dual-Bank HPFP Configuration via CAN Bus (Lua / Python)

*Source: rusEFI Discord, 2025-11-25 | Channel: epicEFI dev (1356732771325968630)*

*Contributors: Tera (teraflopping), ggurov (ggurov)*

## Summary

When running a GDI8 (two GDI4 boards chained together) for a dual-bank high-pressure fuel pump (HPFP) setup, the rusEFI/epicEFI firmware does not natively send pump configuration parameters over CAN to the GDI8 board. That responsibility falls to a Lua script running on the ECU. Because the Lua script space on Proteus is limited (~3,500 characters usable before the GDI4 script exceeds the buffer), the team moved GDI8 configuration delivery to an external Python script that reads tuning parameters from the ECU over CAN and forwards them to the GDI8.

A related issue was discovered: after oscilloscope comparison of the two bank outputs, the second bank's pump was behaving differently because it had never received its configuration from the ECU. The settings appeared to apply only to the primary bank.

## Details

### Lua Space Limitation on Proteus

- Proteus Lua is limited to approximately **3,500 characters** of script.
- The existing GDI4 configuration script already exceeds this limit when comments are included.
- The `.ram4` section showed ~59 kB used with 268 MB total RAM available, meaning the Lua heap could be expanded by ~6 kB, but this was not enough to comfortably fit the full GDI dual-bank script.
- A minification pass (removing comments, shortening variable names) yielded a ~28% reduction (6,474 chars → 4,666 chars in one example), but staying within 3,500 chars for a two-bank script was still problematic.
- Recommendation: keep a human-readable "source" version and a minified "deploy" version.

### Config Not Sent by Core Firmware

- The epicEFI firmware core does **not** transmit GDI pump configuration over CAN on its own.
- The intended mechanism is a Lua script running on the ECU that reads settings from the configuration struct and sends them via `canTx` calls.
- Example Lua snippet for CAN-based GDI communication (minified):

```lua
B=1
A=0xBB20
C=A+0x10
function p()end
function g(d,o,f)return(d[o+2]*256+d[o+1])*f end
function s(d,o,x)if x>1000000000 then return end x=math.floor(x);d[o+2]=x>>8;d[o+1]=x&0xff end
function c1(b,_,_,_)if b~=B then return end end
function c2()end
function c3(_,_,_,d)local x=g(d,6,1/128)setLuaGauge(1,x)end
function c4()end
function v()end
canRxAdd(A,p)
canRxAdd(A+1,c1)
canRxAdd(A+2,c2)
canRxAdd(A+3,c3)
canRxAdd(A+4,c4)
canRxAdd(A+5,v)
T=0x78
local d={T,0,0,0,0,0,0}
F=128
setTickRate(10)
n=0
```

  (Base CAN ID for GDI: `0xBB20`; board index `B=1` for the primary bank.)

### Python as External "Lua" for GDI Config

- Because the Lua space was too tight to fit a two-bank configuration script, the workaround is a Python script running on a separate computer.
- All tuning parameters are exposed over CAN bus (GET and SET); Python can request settings from the ECU and relay them to the GDI8.
- Suggested AI-assisted workflow prompt to generate the script:

  > "Using documentation found in the `epic_can_bus` directory and `variables.json` in the same directory, create a Python script that will send the same variables as the Lua script in `$some_file` by requesting settings from the ECU over CAN bus and sending them over to the GDI8 attached to the same CAN bus."

- TunerStudio itself communicates with the GDI8 via this Lua/CAN pathway; TS does not talk to the GDI8 directly.

### Oscilloscope Diagnosis of Dual-Bank Mismatch

- Tera used a scope to compare pump output waveforms from both banks.
- Changing the peak-time setting from 240° to 130° caused a visible RPM drop, confirming the parameter was reaching the primary bank.
- The secondary bank waveform was different, confirming it was running on default (unconfigured) parameters.
- Root cause hypothesis: the second bank never received its configuration because the Lua script either ran out of space before handling bank 2, or the script only targeted bank 1.

### Adding a Second Feedback Loop / Offset for Bank 2

- Long-term plan: implement a full second feedback control loop for the second HPFP bank.
- Short-term workaround: add a static offset for the second bank's control parameters.
- A PR was opened on `epicEFI/epicefi_fw` to add a `highPressureFuel2` sensor struct alongside the existing `highPressureFuel` in the configuration.

### INI File Workflow for New Fields

- When adding new fields to the firmware (e.g., `highPressureFuel2`), the generated INI must be updated in TunerStudio.
- After building with `./build_proteus_f4_bundle.sh`, verify the generated file:
  ```
  nano tunerstudio/generated/rusefi_proteus_f4.ini
  # Ctrl-W to search for "highpressure" — confirm "highPressureFuel2" appears
  ```
- Also check `./integration/rusefi_config.txt` for the struct definition:
  ```
  linear_sensor_s highPressureFuel;
  linear_sensor_s lowPressureFuel;
  // highPressureFuel2 should appear here after the PR is merged
  ```
- TunerStudio hidden diagnostic: **Ctrl-Shift-E** opens an expression evaluator where you can paste a variable name (e.g., `highPressureFuel2_value1`) to verify it is present in the loaded INI.
- TunerStudio hidden tool: **Ctrl-Shift-F** performs float ↔ hex conversion (noted as partially broken).
- Always accept the INI update prompt when reloading firmware to ensure the new fields appear in menus.

### Docker / WSL2 Build Environment Tip

- When building inside a Docker container on Windows, the Cursor IDE terminal's `pwd` and the Windows filesystem explorer path will differ.
- The `rusefibuildcontainer` mounts the host `repo/` directory as `/rusefi/` inside the container.
- Recommended workflow: clone the repo inside WSL2 (not in the Windows filesystem), then connect Cursor to WSL2 over SSH or use the WSL extension:
  ```
  # In WSL2:
  sudo apt-get -y install openssh-server
  # Connect Cursor to WSL2 via Remote-SSH or the WSL plugin
  ```
- This eliminates the path mismatch between the container view and the editor view.

### GDI8 Hardware Background

- GDI8 is architecturally two GDI4 boards combined; the design originated from a GDI4 that was doubled for V8 use.
- The GDI4 script (one bank) is the building block; a dual-bank script must manage both banks independently via CAN.
- The original GDI8 implementation (by Andrey/nozx) was tested on a V8 CAMAROW DI application; it is speculated that only a single HPFP was used in that setup, avoiding the two-bank config problem.
