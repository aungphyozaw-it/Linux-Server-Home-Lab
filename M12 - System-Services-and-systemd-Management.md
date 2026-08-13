# M12 — System Services & systemd Management

## Objective

Learn how to manage, monitor, troubleshoot, enable, disable, start, stop, restart, and inspect Linux services using `systemd`.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Check Service Status

### Commands

`systemctl status ssh`  
`systemctl is-active ssh`  
`systemctl is-enabled ssh`

### Key Points

- `status` — Check service status and recent logs.
- `is-active` — Check whether the service is currently running.
- `is-enabled` — Check whether the service starts automatically at boot.

---

## 2. Start / Stop / Restart Services

### Commands

`sudo systemctl start <service>`  
`sudo systemctl stop <service>`  
`sudo systemctl restart <service>`

### Example

`sudo systemctl restart ssh`

### Key Points

- `start` — Start a stopped service.
- `stop` — Stop a running service.
- `restart` — Restart the service.

---

## 3. Reload Service Configuration

### Commands

`sudo systemctl reload <service>`  
`sudo systemctl daemon-reload`

### Difference

- `reload` — Reload the service configuration without fully stopping the service, if supported.
- `daemon-reload` — Tell systemd to reload unit files after they have been changed.

---

## 4. Enable / Disable Services

### Commands

`sudo systemctl enable <service>`  
`sudo systemctl disable <service>`

### Enable and Start Together

`sudo systemctl enable --now <service>`

### Disable and Stop Together

`sudo systemctl disable --now <service>`

### Key Point

`enable` controls whether a service starts automatically during boot.

---

## 5. List Running Services

### Commands

`systemctl list-units --type=service`  
`systemctl list-units --type=service --state=running`

### Key Point

Useful for checking which services are currently loaded and running.

---

## 6. Failed Services

### Commands

`systemctl --failed`  
`systemctl list-units --failed`

### Key Point

This is one of the first commands to use when checking a server for service problems.

---

## 7. Service Logs

### Commands

`journalctl -u <service>`  
`journalctl -u <service> -b`  
`journalctl -u <service> --since "1 hour ago"`

### Example

`journalctl -u ssh -b`

### Key Point

Use service-specific logs to investigate why a service failed or behaves unexpectedly.

---

## 8. Real-Time Service Logs

### Command

`journalctl -u <service> -f`

### Example

`journalctl -u ssh -f`

### Key Point

`-f` follows new log entries in real time.

---

## 9. Service Unit Information

### Commands

`systemctl cat <service>`  
`systemctl show <service>`

### Key Point

Useful for checking how a service is configured and how systemd manages it.

---

## 10. Service Dependencies

### Commands

`systemctl list-dependencies <service>`  
`systemctl list-dependencies --reverse <service>`

### Key Point

Useful when one service depends on another service.

---

## 11. Check System Boot Target

### Commands

`systemctl get-default`  
`systemctl list-units --type=target`

### Key Point

The default target determines the normal system boot mode.

---

## 12. System Shutdown / Reboot

### Commands

`sudo systemctl reboot`  
`sudo systemctl poweroff`

### Key Point

Use these commands when performing controlled server maintenance.

---

## 13. Service Troubleshooting Workflow

### Scenario

A service is not working.

### Step 1 — Check Status

`systemctl status <service>`

### Step 2 — Check Active State

`systemctl is-active <service>`

### Step 3 — Check Logs

`journalctl -u <service> -b`

### Step 4 — Check Failed Services

`systemctl --failed`

### Step 5 — Check Listening Ports

`sudo ss -tulpn`

### Step 6 — Restart if Appropriate

`sudo systemctl restart <service>`

### Step 7 — Verify

`systemctl status <service>`  
`systemctl is-active <service>`

---

## 14. SSH Service — Real Lab Example

### Check

`systemctl status ssh`

### Start

`sudo systemctl start ssh`

### Stop

`sudo systemctl stop ssh`

### Restart

`sudo systemctl restart ssh`

### Enable at Boot

`sudo systemctl enable ssh`

### Check

`systemctl is-enabled ssh`  
`systemctl is-active ssh`

### Check Port

`sudo ss -tulpn | grep :22`

---

## 15. Important Commands Summary

### Service Management

`systemctl status <service>`  
`sudo systemctl start <service>`  
`sudo systemctl stop <service>`  
`sudo systemctl restart <service>`  
`sudo systemctl reload <service>`

### Boot Configuration

`sudo systemctl enable <service>`  
`sudo systemctl disable <service>`  
`sudo systemctl enable --now <service>`

### Troubleshooting

`systemctl --failed`  
`journalctl -u <service> -b`  
`systemctl cat <service>`  
`systemctl show <service>`

### System

`systemctl get-default`  
`sudo systemctl reboot`  
`sudo systemctl poweroff`

---

## Real-Work Troubleshooting Flow

Service Problem  
↓  
`systemctl status <service>`  
↓  
`systemctl is-active <service>`  
↓  
`journalctl -u <service> -b`  
↓  
Check configuration / dependencies  
↓  
Restart or fix the service  
↓  
Verify service status  
↓  
Verify application / port connectivity

---

## Result

Successfully studied the main Linux `systemd` and service-management operations required for Linux Server administration.

## Module Status

**M11 — COMPLETED** ✅
