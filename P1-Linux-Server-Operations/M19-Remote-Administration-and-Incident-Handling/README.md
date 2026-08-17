# M19 — Remote Administration & Incident Handling

## Objective

Learn how to securely administer Linux servers remotely and handle operational incidents using a structured investigation, recovery, verification, and documentation process.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Windows Host
- SSH
- Linux Server Home Lab

---

## 1. Check Server Reachability

### Commands

`ping <server-ip>`

`ip route`

### Key Point

Before attempting remote administration, confirm basic network reachability.

---

## 2. Test SSH Connectivity

### Command

`ssh <user>@<server-ip>`

### Example

`ssh aung@192.168.176.128`

### Key Point

Confirm that the server is reachable and SSH authentication works.

---

## 3. Verify SSH Port

### Commands

`nc -zv <server-ip> 22`

`sudo ss -tulpn | grep :22`

### Check

- Server reachability
- SSH port availability
- Listening service

---

## 4. Remote System Information

### Commands

`hostname`

`hostnamectl`

`uname -r`

`uptime`

`df -h`

`free -h`

### Key Point

After connecting remotely, collect basic system information before troubleshooting.

---

## 5. Check Remote User & Privileges

### Commands

`whoami`

`id`

`groups`

`sudo -l`

### Key Point

Confirm which account is being used and whether the required administrative privileges are available.

---

## 6. Remote Service Administration

### Commands

`systemctl status <service>`

`sudo systemctl restart <service>`

`systemctl is-active <service>`

### Verify

`systemctl status <service>`

### Key Point

Always verify the service after performing a remote administrative action.

---

## 7. Remote Log Investigation

### Commands

`journalctl -u <service> -b`

`journalctl -p err -b`

`journalctl --since "1 hour ago"`

### Key Point

Use logs as evidence when investigating a remote incident.

---

## 8. Remote Network Troubleshooting

### Commands

`ip -br addr`

`ip route`

`ping <gateway-ip>`

`ping 8.8.8.8`

`resolvectl query google.com`

### Key Point

Determine whether the problem is related to:

- Interface
- IP address
- Gateway
- DNS
- Internet connectivity

---

## 9. Remote Storage Check

### Commands

`df -h`

`df -i`

`lsblk`

### Key Point

A full filesystem or exhausted inodes can cause applications and services to fail.

---

## 10. Remote Process Investigation

### Commands

`ps aux --sort=-%cpu | head`

`ps aux --sort=-%mem | head`

`pgrep -a <process-name>`

### Key Point

Identify abnormal resource-consuming processes before taking action.

---

## 11. Incident Identification

### Scenario

A remote Linux server is reported as having an application outage.

### First Actions

`uptime`

`systemctl --failed`

`df -h`

`free -h`

`ip -br addr`

### Key Point

Start with a quick health assessment before making changes.

---

## 12. Incident Evidence Collection

### Collect

`date`

`hostname`

`uptime`

`who`

`last`

`systemctl --failed`

`df -h`

`free -h`

`ip -br addr`

`ip route`

### Logs

`journalctl -p err -b`

`journalctl --since "1 hour ago"`

### Key Point

Collect evidence before making major changes whenever possible.

---

## 13. Incident Classification

Determine whether the incident is primarily related to:

### Service

`systemctl status <service>`

### Application

`journalctl -u <service> -b`

### Network

`ip route`

`ping <gateway-ip>`

### Storage

`df -h`

### Memory

`free -h`

### Process

`ps aux --sort=-%cpu | head`

### Security / Access

`last`

`journalctl -u ssh -b`

---

## 14. Incident Containment

### Purpose

Prevent the problem from becoming worse while investigating.

### Examples

- Stop a failing non-critical process.
- Prevent unnecessary service restarts.
- Restrict unnecessary network access.
- Preserve important logs.
- Avoid destructive changes.

### Key Point

Containment should be controlled and reversible whenever possible.

---

## 15. Controlled Recovery

### Example — Restart Failed Service

`sudo systemctl restart <service>`

### Verify

`systemctl is-active <service>`

### Check Logs

`journalctl -u <service> -b`

### Key Point

Recovery actions should be based on evidence rather than guesswork.

---

## 16. Remote File Transfer

### Copy File From Server

`scp <user>@<server-ip>:/path/to/file .`

### Copy File To Server

`scp <file> <user>@<server-ip>:/path/to/destination/`

### Key Point

Useful for transferring configuration files, logs, scripts, and evidence.

---

## 17. Remote Command Execution

### Command

`ssh <user>@<server-ip> "<command>"`

### Example

`ssh aung@192.168.176.128 "uptime"`

### Key Point

Useful for quick remote checks without starting an interactive shell.

---

## 18. Incident Verification

After recovery, verify:

### Service

`systemctl is-active <service>`

### Port

`sudo ss -tulpn | grep :<port>`

### Network

`ping <gateway-ip>`

### Resources

`free -h`

`df -h`

### Application

`curl -I http://127.0.0.1:<port>`

### Logs

`journalctl -u <service> -b`

### Key Point

Recovery is not complete until the original problem has been verified as resolved.

---

## 19. Incident Documentation

### Record

- Incident date/time
- Affected server
- Symptoms
- User impact
- Initial observations
- Evidence collected
- Investigation steps
- Root cause
- Corrective action
- Verification
- Final result

### Example Structure

**Incident:** Application unavailable

**Impact:** Application service unavailable to users

**Investigation:** Service status, logs, port, disk space

**Root Cause:** Filesystem reached critical capacity

**Fix:** Removed/rotated confirmed unnecessary data

**Verification:** Service restored and application responded normally

**Result:** Incident resolved

---

## 20. Remote Administration & Incident Workflow

Remote Incident  
↓  
Confirm Server Reachability  
↓  
`ping <server-ip>`  
↓  
Connect Using SSH  
↓  
`ssh <user>@<server-ip>`  
↓  
Collect Initial Evidence  
↓  
`uptime` / `df -h` / `free -h`  
↓  
Check Services  
↓  
`systemctl --failed`  
↓  
Check Logs  
↓  
`journalctl -p err -b`  
↓  
Check Network / Storage / Process  
↓  
Identify Root Cause  
↓  
Contain Incident  
↓  
Apply Controlled Recovery  
↓  
Verify Service / Network / Application  
↓  
Document Incident  
↓  
Close Incident

---

## Important Commands Summary

### Remote Access

`ssh <user>@<server-ip>`

`scp <file> <user>@<server-ip>:/path/`

`ssh <user>@<server-ip> "<command>"`

### Server Health

`uptime`

`free -h`

`df -h`

`lsblk`

### Service

`systemctl status <service>`

`systemctl --failed`

`sudo systemctl restart <service>`

### Logs

`journalctl -p err -b`

`journalctl -u <service> -b`

### Network

`ip -br addr`

`ip route`

`ping <gateway-ip>`

### Process

`ps aux --sort=-%cpu | head`

`ps aux --sort=-%mem | head`

### Access Investigation

`who`

`last`

`journalctl -u ssh -b`

---

## Result

Successfully studied the main remote administration and incident-handling procedures required for Linux server operations.

The module covered:

- Remote SSH administration
- Remote system health checks
- Remote service management
- Remote log investigation
- Remote network troubleshooting
- Remote storage checks
- Process investigation
- Incident identification
- Evidence collection
- Incident classification
- Controlled containment
- Recovery
- Verification
- Remote file transfer
- Incident documentation

## Module Status

**M19 — COMPLETED** ✅
