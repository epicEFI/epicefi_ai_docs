# Reading O2 Sensor Data via OBD-II Over CAN Using Lua

*Source: rusEFI Discord, 2025-11-23 | Channel: 1356732771325968630*
*Contributors: ggurov (@ggurov), Tera (@teraflopping), Joesphan (@joesphan)*

## Summary

When a stock ECU retains connected O2 sensors (as in the BMW S58 project discussed here), the epicEFI ECU can retrieve O2 sensor readings by sending OBD-II PID requests over CAN via Lua and listening for the reply frames. This avoids needing direct wiring to the O2 sensor signals.

## Details

### Background

The stock ECU on this project vehicle (BMW S58 engine) keeps its O2 sensors connected and responds to standard OBD-II requests over the OBD2 connector. The epicEFI unit needed access to those values. The stock ECU does not broadcast O2 data on the CAN bus unsolicited — it must be explicitly queried.

TunerStudio cannot feed that data back to the ECU, so the solution is a Lua script running on the ECU itself.

### Approach

1. Connect the epicEFI CAN bus to the vehicle's OBD-II CAN network (the same bus the stock ECU is on).
2. Write a Lua script that:
   - Sends the OBD-II Mode 01 PID request frame to the stock ECU.
   - Registers a `canRxAddMask` listener for the reply frame.
   - Parses the reply and feeds the value to a virtual sensor.

### Lua Structure (ggurov's example pattern)

```lua
-- Poll at 2 Hz (start conservative; can increase once validated)
setTickRate(2)

-- Listen for standard OBD-II reply range 0x7E8–0x7EF on CAN bus 1
canRxAddMask(1, 0x7E8, 0x7F8)

local function request_o2()
    -- Mode 01, PID 0x14 = O2 Sensor Bank 1 Sensor 1 (voltage + STFT)
    -- Standard SAE OBD-II request frame: [len=2, mode=01, pid=0x14, padding...]
    canTx(1, 0x7DF, 8, 0x02, 0x01, 0x14, 0x00, 0x00, 0x00, 0x00, 0x00)
end

function onTick()
    request_o2()
end

function onCanRx(bus, id, dlc, data)
    -- Reply from ECU at 0x7E8 (or 0x7E9–0x7EF for other modules)
    -- Byte 2 = mode+0x40 (0x41), Byte 3 = PID (0x14)
    -- Byte 4 = O2 voltage (V = byte4 * 0.005), Byte 5 = STFT %
    if id == 0x7E8 and data[3] == 0x41 and data[4] == 0x14 then
        local volts = data[5] * 0.005
        -- feed into virtual sensor or output variable
    end
end
```

### Notes

- **Tick rate**: 2 Hz is a conservative starting point. Standard OBD-II response time is typically under 50 ms, but polling at high rates on a shared bus can cause contention.
- **Extended addressing**: For vehicles like the C8 Corvette that use 29-bit extended OBD-II addresses (0x18DAF111), the `canRxAddMask` filter and request target CAN ID must be changed accordingly. See the separate doc on extended OBD-II addressing.
- **BMW/UDS caveat**: The S58 stock ECU may use BMW-specific UDS (Unified Diagnostic Services) rather than generic SAE OBD-II for some sensor data. If standard Mode 01 PID requests do not return O2 data, BMW UDS service 0x22 with the appropriate data identifier will be needed.
- **Alternative**: The Vehical (external CAN logger) app can read the O2 values directly from ECU RAM without OBD-II queries. If correlating logs post-hoc is acceptable, syncing a Vehical log with TunerStudio log by timestamp is an alternative to the real-time Lua approach.

### Related PIDs (SAE OBD-II Mode 01)

| PID  | Description                              |
|------|------------------------------------------|
| 0x14 | O2 Sensor Bank 1 Sensor 1 (V + STFT)    |
| 0x15 | O2 Sensor Bank 1 Sensor 2 (V + STFT)    |
| 0x24 | O2 Sensor Bank 1 Sensor 1 (wideband, equivalence ratio + current) |
| 0x25 | O2 Sensor Bank 1 Sensor 2 (wideband)    |
