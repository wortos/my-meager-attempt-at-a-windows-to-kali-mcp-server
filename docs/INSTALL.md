# Installation Guide

This guide shows one way to connect Claude Desktop on Windows to a Kali MCP server over SSH.

## 1. Confirm SSH works from Windows to Kali

### What this check does

This command tests basic SSH connectivity and verifies that the remote machine accepts a login and can run a simple command.

```powershell
ssh user@KALI-IP "echo OK"
```

You should get:

```text
OK
```

If not, fix SSH or networking first.

## 2. Verify that mcp-server exists on Kali

### What this check does

This command asks Kali where `mcp-server` is installed. It confirms that the executable is present and discoverable in PATH.

```powershell
ssh user@KALI-IP "which mcp-server"
```

Expected output might look like:

```text
/usr/bin/mcp-server
```

## 3. Locate Claude Desktop's config file on Windows

Depending on your installation, check one of these:

### Microsoft Store installation

```text
%LOCALAPPDATA%\Packages\Claude_pzs8sxrjxfjjc\LocalCache\Roaming\Claude\claude_desktop_config.json
```

### Standard installation

```text
%APPDATA%\Claude\claude_desktop_config.json
```

## 4. Add an MCP server entry

Use the example in `examples/claude_desktop_config.example.json` and adapt it to your environment.

Important:

- Replace the SSH key path
- Replace the username
- Replace the Kali IP
- Do not commit private keys

## 5. Restart Claude Desktop fully

### What this step is verifying

You are making sure Claude reloads the configuration file from disk instead of continuing to use cached settings.

Close Claude completely, then reopen it.

## 6. Test a minimal remote workflow

Start with a small command before trying scans or long-running actions.

### What this verifies

You are checking that:

- the config is valid
- SSH launches correctly
- Claude can start the remote process
- the server responds

## 7. Move to longer commands only after basic success

If simple checks work but scans fail, review:

- timeout settings
- sudo or permission issues
- network reachability to scan targets
- command execution limits in the MCP wrapper

## Optional Kali checks

Run these directly on Kali if you suspect a network problem.

### Show IP addresses

```bash
ip addr
```

### Show network devices

```bash
nmcli device status
```

### Show configured connections

```bash
nmcli connection show
```

### Check NetworkManager

```bash
systemctl status NetworkManager
```
