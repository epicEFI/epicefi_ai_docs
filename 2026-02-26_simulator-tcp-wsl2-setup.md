# Running the rusEFI Simulator over TCP in WSL2

*Source: epicEFI Discord, 2026-02-26 | Channel: 1440539911059931259*
*Contributors: @ggurov, @SanGawku*

## Summary

Instructions for running the rusEFI TCP simulator inside WSL2 and connecting to it from Windows. The simulator listens on localhost within WSL2, so a port-forwarding approach is needed when connecting from the Windows host or from TunerStudio. Additionally, a Python/C bridge exists for proxying TCP traffic to the CAN bus.

## Details

### Starting the Simulator

1. Unzip the simulator binary.
2. Open a WSL2 terminal and run:
   ```bash
   ./rusefi_simulator 0>/dev/null
   ```
3. The simulator will output:
   ```
   [sim] TCP TS: listening on port 29002
   [sim] TCP TS: listening on port 29001
   ```
   - Port 29001: primary TCP TS port
   - Port 29002: secondary TCP TS port

### Networking: WSL2 Localhost vs. Windows Host

- The simulator listens on the WSL2 internal network address (a 172.x.x.x address), **not** on `0.0.0.0`.
- To connect from Windows, the WSL2 IP must be used. Find it inside WSL2 with:
  ```bash
  ip a
  ```
  Look for the `eth0` interface, e.g.:
  ```
  inet 172.27.85.235/20 brd 172.27.95.255 scope global eth0
  ```
- Connect TunerStudio (or any TCP client) to `172.27.85.235:29001` from Windows.

### Alternative: SSH Port Forwarding

If the WSL2 address is not directly reachable, use an SSH local tunnel from Windows:

```cmd
ssh -L 8888:<wsl2-ip>:29001 <user>@<wsl2-host>
```

Then connect TunerStudio to `localhost:8888`.

### TCP-to-CAN Bus Bridge

- @ggurov has written a bridge (in Python and C) that proxies TCP connections through to the CAN bus.
- This allows the ECU to communicate over TCP while the physical interface is CAN.
- The TCP feature was still under active development/debugging at the time of this conversation.
