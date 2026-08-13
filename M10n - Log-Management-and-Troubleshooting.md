# M10 — Log Management & Troubleshooting

## Objective

Learn how to read, filter, monitor, and troubleshoot Linux system and service logs using `journalctl`, `dmesg`, and common log files.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. View System Logs

### Commands

`journalctl`  
`journalctl -b`  
`journalctl -p err`  
`journalctl -p warning`

### Key Points

- `journalctl` — View systemd journal logs.
- `journalctl -b` — Show logs from the current boot.
- `journalctl -p err` — Show error-level messages.
- `journalctl -p warning` — Show warning-level messages.

---

## 2. View Service Logs

### Commands

`journalctl -u <service>`  
`journalctl -u <service> -b`  
`journalctl -u <service> --since "1 hour ago"`

### Example

`journalctl -u ssh -b`

### Key Point

Use service-specific logs when a service fails, stops, or behaves unexpectedly.

---

## 3. Real-Time Log Monitoring

### Commands

`journalctl -f`  
`journalctl -u ssh -f`

### Key Point

`-f` follows new log entries in real time.

Useful when troubleshooting a service while reproducing the problem.

---

## 4. Filter Logs by Time

### Commands

`journalctl --since "1 hour ago"`  
`journalctl --since today`  
`journalctl --since yesterday`  
`journalctl --since "2026-08-01 10:00:00" --until "2026-08-01 12:00:00"`

### Key Point

Time filtering helps reduce a large amount of log data during troubleshooting.

---

## 5. Filter Logs by Priority

### Commands

`journalctl -p err`  
`journalctl -p warning`  
`journalctl -p 0..3`

### Common Priorities

`0` — Emergency  
`1` — Alert  
`2` — Critical  
`3` — Error  
`4` — Warning  
`5` — Notice  
`6` — Informational  
`7` — Debug

---

## 6. Kernel Logs

### Commands

`dmesg`  
`dmesg | tail -50`  
`dmesg | grep -i error`  
`dmesg | grep -i "I/O"`

### Useful For

- Hardware problems
- Disk errors
- Network interface problems
- Driver issues
- Kernel errors
- Boot-related problems

---

## 7. Traditional Log Files

### Important Locations

`/var/log/`

### Common Files

`/var/log/syslog`  
`/var/log/auth.log`  
`/var/log/kern.log`

### Commands

`sudo less /var/log/syslog`  
`sudo less /var/log/auth.log`  
`sudo less /var/log/kern.log`

### Key Point

Some applications and services still write directly to files under `/var/log/`.

---

## 8. Search Logs

### Commands

`grep "error" /var/log/syslog`  
`grep -i "error" /var/log/syslog`  
`grep -i "failed" /var/log/auth.log`  
`journalctl | grep -i error`

### Key Point

`grep -i` performs a case-insensitive search.

---

## 9. Follow Traditional Log Files

### Commands

`sudo tail -f /var/log/syslog`  
`sudo tail -f /var/log/auth.log`

### Key Point

Useful for watching new log entries while reproducing a problem.

---

## 10. Authentication Troubleshooting

### Log

`/var/log/auth.log`

### Commands

`sudo grep -i "failed" /var/log/auth.log`  
`sudo grep -i "accepted" /var/log/auth.log`  
`sudo tail -f /var/log/auth.log`

### Useful For

- SSH login failures
- Successful logins
- Authentication problems
- sudo activity

---

## 11. Boot Troubleshooting

### Commands

`journalctl -b`  
`journalctl -b -p err`  
`journalctl -k -b`  
`systemctl --failed`

### Key Point

These commands are useful when a server boots slowly, services fail during boot, or hardware problems occur during startup.

---

## 12. Log Storage Usage

### Commands

`journalctl --disk-usage`  
`sudo du -sh /var/log`

### Key Point

Logs can consume significant disk space on long-running servers.

---

## 13. Journal Cleanup

### Commands

`sudo journalctl --vacuum-time=7d`  
`sudo journalctl --vacuum-size=500M`

### Important

Only remove logs according to your organization's retention policy.

Never delete logs blindly on production systems.

---

## 14. Troubleshooting Workflow

### Step 1 — Confirm the Problem

Identify:

- What failed?
- When did it fail?
- Which service or system component is affected?

### Step 2 — Check Service Status

`systemctl status <service>`

### Step 3 — Check Service Logs

`journalctl -u <service> -b`

### Step 4 — Check System Errors

`journalctl -p err`

### Step 5 — Check Kernel Messages

`dmesg | tail -50`

### Step 6 — Search Relevant Logs

`grep -i "error" /var/log/syslog`

### Step 7 — Fix the Root Cause

Apply the appropriate corrective action.

### Step 8 — Verify

`systemctl status <service>`  
`journalctl -u <service> -b`

---

## 15. Real-Work Scenario — SSH Failure

### Scenario

SSH connection to the Ubuntu Server fails.

### Check Service

`systemctl status ssh`

### Check Port

`sudo ss -tulpn | grep :22`

### Check Authentication Logs

`sudo grep -i "failed" /var/log/auth.log`

### Check SSH Logs

`journalctl -u ssh -b`

### Check Network

`ip addr`  
`ip route`  
`ping 8.8.8.8`

### Verification

`systemctl is-active ssh`  
`sudo ss -tulpn | grep :22`

---

## 16. Real-Work Scenario — Disk Error

### Check Kernel Logs

`dmesg | grep -i error`  
`dmesg | grep -i "I/O"`

### Check System Logs

`journalctl -k -b`

### Check Disk

`lsblk -f`  
`df -h`

### Check Disk Health

`sudo smartctl -a /dev/sda`

---

## 17. Important Log Commands Summary

### System

`journalctl`  
`journalctl -b`  
`journalctl -p err`

### Service

`journalctl -u <service>`  
`journalctl -u <service> -b`  
`journalctl -u <service> -f`

### Kernel

`dmesg`  
`dmesg | tail -50`  
`journalctl -k -b`

### Traditional Logs

`sudo less /var/log/syslog`  
`sudo less /var/log/auth.log`  
`sudo less /var/log/kern.log`

### Search

`grep -i "error" /var/log/syslog`  
`grep -i "failed" /var/log/auth.log`

### Monitoring

`sudo tail -f /var/log/syslog`  
`sudo tail -f /var/log/auth.log`

### Log Storage

`journalctl --disk-usage`  
`sudo du -sh /var/log`

---

## Real-Work Log Troubleshooting Flow

Problem  
↓  
Identify time and affected service  
↓  
`systemctl status <service>`  
↓  
`journalctl -u <service> -b`  
↓  
`journalctl -p err`  
↓  
`dmesg | tail -50`  
↓  
Search `/var/log/` if required  
↓  
Identify root cause  
↓  
Apply fix  
↓  
Verify logs and service status

---

## Result

Successfully studied the main Linux log-management and troubleshooting techniques required for server operations.

The module covered:

- system logs
- service logs
- kernel logs
- authentication logs
- log filtering
- time-based searches
- real-time monitoring
- `journalctl`
- `dmesg`
- `grep`
- `tail`
- log storage
- basic log cleanup
- SSH troubleshooting
- disk-error troubleshooting

## Module Status

**M12 — COMPLETED** ✅
