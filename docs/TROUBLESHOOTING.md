# Troubleshooting

![Docs](https://img.shields.io/badge/doc-troubleshooting-1f6feb)
![Focus](https://img.shields.io/badge/focus-networking%20%26%20SSH-2ea043)
![Client](https://img.shields.io/badge/client-Claude%20Desktop-8250df)
![Host](https://img.shields.io/badge/host-Kali-orange)

This page covers the failure points most likely to break a **Windows-to-Kali MCP setup**.

## Triage flow

```mermaid
flowchart TD
    A[Problem] --> B{Does SSH work?}
    B -- No --> C[Check IP, SSH service, key path, VM networking]
    B -- Yes --> D{Does mcp-server exist?}
    D -- No --> E[Install or fix PATH on Kali]
    D -- Yes --> F{Does Claude load the config?}
    F -- No --> G[Check the active config path and restart Claude]
    F -- Yes --> H{Do only long actions fail?}
    H -- Yes --> I[Review timeouts, prompts, permissions, reachability]
    H -- No --> J[Re-check config syntax and workflow assumptions]
```

## Claude Desktop ignores config changes

### Possible cause

Claude Desktop may be reading the Microsoft Store sandboxed config path instead of the roaming profile path.

### Check

```text
%LOCALAPPDATA%\Packages\Claude_pzs8sxrjxfjjc\LocalCache\Roaming\Claude\claude_desktop_config.json
```

```text
%APPDATA%\Claude\claude_desktop_config.json
```

### Fix

- edit the active file
- save it
- fully restart Claude Desktop

## SSH from Windows to Kali fails

### Checks

```bash
ip addr
```

```powershell
ssh user@KALI-IP "echo OK"
```

### Things to verify

- Kali is powered on
- the SSH service is installed and running
- the VM network mode is working
- the private key path is correct
- the target IP is the current one, not an old cached value

## Network interface issues in Kali

### Checks

```bash
nmcli device status
nmcli connection show
```

### Possible fixes

- review `/etc/network/interfaces`
- review `/etc/NetworkManager/NetworkManager.conf`
- remove conflicting old connections
- enable autoconnect for the correct connection
- restart NetworkManager

```bash
systemctl restart NetworkManager
```

## Specific case: `eth0` shows as unmanaged

### Symptom

`nmcli device status` shows the interface as `unmanaged`, which means NetworkManager will not apply connection changes to it yet.

### Fix

First tell NetworkManager to manage the device:

```bash
sudo nmcli device set eth0 managed yes
```

Then verify that it is now managed:

```bash
nmcli device status
```

After that, you can bring up or modify the connection normally, for example:

```bash
sudo nmcli connection up "HOSTONLY-CONNECTION"
sudo nmcli connection modify "HOSTONLY-CONNECTION" ipv4.addresses 192.168.56.101/24 ipv4.method manual
```

Replace:

- `eth0` with the actual device name if different
- `HOSTONLY-CONNECTION` with the actual connection name

## Specific case: SSH banner or MOTD breaks MCP startup

### Symptom

SSH connects, but the MCP session drops almost immediately or behaves as if the remote process exits after only a very short time.

### Likely cause

Extra stdout text from banners, MOTD scripts, shell startup files, or other login-time output can interfere with an MCP stdio session.

### Things to check

Review whether the SSH login prints anything before `mcp-server` starts, such as:

- MOTD output
- custom shell echo statements
- banner text
- profile scripts that write to stdout

### Fix ideas

- remove or silence unnecessary login-time output for the SSH session used by Claude
- test the exact SSH command manually and confirm it does not emit extra text before `mcp-server`
- keep the MCP SSH path as quiet and non-interactive as possible

## Short tasks work but longer actions fail

### Possible cause

A timeout may be enforced in the server wrapper or command runner.

### Things to review

- timeout values in the server-side code
- network reachability
- permissions
- commands waiting on interactive prompts
- workflows that require elevated privileges

## Recommended order

1. Verify the Kali IP
2. Verify SSH from Windows
3. Verify `mcp-server` exists
4. Verify the Claude config path
5. Restart Claude Desktop fully
6. Test a simple remote command first
7. Only then test longer actions

## Privacy reminder

Do not paste real usernames, real IP addresses, key filenames, or raw environment logs into public troubleshooting notes.
