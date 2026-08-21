# M20 — End-to-End Linux Server Troubleshooting

## Objective

Perform a complete end-to-end Linux Server troubleshooting exercise by following a structured workflow from incident identification through investigation, root-cause analysis, recovery, verification, and technical documentation.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Windows Host
- Linux Server Home Lab
- SSH

---

## 1. Incident Scenario

### Scenario

A Linux server is experiencing multiple operational problems.

Possible symptoms:

- Application unavailable
- Service failure
- High resource usage
- Network connectivity problem
- Storage capacity problem
- SSH access issue

The objective is to investigate the incident systematically rather than applying random fixes.

---

## 2. Step 1 — Confirm the Incident

### Commands

`date`

`hostname`

`uptime`

`who`

`w`

### Check

- Affected server
- Current time
- System uptime
- Logged-in users
- Current system state

### Key Point

Confirm the incident and establish the initial system state before making changes.

---

## 3. Step 2 — Check Overall System Health

### Commands

`uptime`

`free -h`

`df -h`

`df -i`

`systemctl --failed`

### Purpose

Identify obvious:

- CPU/load problems
- Memory pressure
- Disk-space problems
- Inode exhaustion
- Failed services

---

## 4. Step 3 — Investigate Processes

### Commands

`ps aux --sort=-%cpu | head`

`ps aux --sort=-%mem | head`

`pgrep -a <process-name>`

`top`

### Check

- High CPU processes
- High memory processes
- Unexpected processes
- Application processes

### Key Point

Do not terminate a process until its role and impact are understood.

---

## 5. Step 4 — Investigate Services

### Commands

`systemctl --failed`

`systemctl status <service>`

`systemctl is-active <service>`

### Check

- Failed services
- Stopped services
- Restart loops
- Service state

---

## 6. Step 5 — Investigate Logs

### Commands

`journalctl -p err -b`

`journalctl -u <service> -b`

`journalctl --since "1 hour ago"`

### Key Point

Use logs as evidence to identify what happened before changing the system.

---

## 7. Step 6 — Investigate Network

### Commands

`ip -br link`

`ip -br addr`

`ip route`

`ping <gateway-ip>`

`ping 8.8.8.8`

`resolvectl query google.com`

### Check

- Interface state
- IP configuration
- Default route
- Gateway connectivity
- Internet connectivity
- DNS resolution

---

## 8. Step 7 — Investigate Ports

### Commands

`sudo ss -tulpn`

`sudo ss -tulpn | grep :<port>`

`nc -zv <server-ip> <port>`

### Check

- Expected listening port
- Application port
- Unexpected open ports
- Port accessibility

---

## 9. Step 8 — Investigate Storage

### Commands

`lsblk`

`lsblk -f`

`df -h`

`df -i`

`findmnt`

### Find Large Data

`sudo du -xhd1 / 2>/dev/null | sort -h`

### Check Deleted Files

`sudo lsof +L1`

### Key Point

Determine whether the incident is related to capacity, filesystem, mount, or disk I/O.

---

## 10. Step 9 — Investigate Disk Performance

### Commands

`iostat -xz 1 5`

`vmstat 1 5`

### Check

- I/O wait
- Device utilisation
- Read/write activity
- System activity

---

## 11. Step 10 — Identify Root Cause

Organise the evidence collected.

### Possible Root Causes

- Failed service
- Incorrect configuration
- High CPU usage
- Memory pressure
- Disk full
- Inode exhaustion
- Filesystem issue
- Network configuration problem
- DNS failure
- Port conflict
- Application failure
- Dependency failure

### Key Point

Do not confuse the symptom with the root cause.

---

## 12. Step 11 — Containment

### Objective

Prevent the incident from becoming worse.

### Possible Actions

- Stop a non-critical failing process.
- Prevent repeated service restarts.
- Restrict unnecessary access.
- Preserve important logs.
- Avoid destructive storage operations.

### Key Point

Containment should be controlled and reversible whenever possible.

---

## 13. Step 12 — Apply Corrective Action

### Examples

Restart a failed service:

`sudo systemctl restart <service>`

Reload configuration:

`sudo systemctl reload <service>`

Correct a configuration problem using the application's supported configuration method.

Recover storage space only after confirming which data is safe to remove or rotate.

Correct network configuration only after identifying the actual networking problem.

### Key Point

Apply the smallest appropriate fix based on evidence.

---

## 14. Step 13 — Verify Recovery

### Service

`systemctl is-active <service>`

### Port

`sudo ss -tulpn | grep :<port>`

### Network

`ping <gateway-ip>`

### DNS

`resolvectl query google.com`

### Storage

`df -h`

### Memory

`free -h`

### Application

`curl -I http://127.0.0.1:<port>`

### Logs

`journalctl -u <service> -b`

### Key Point

The incident is not resolved until the original symptom has been verified as fixed.

---

## 15. Step 14 — Confirm No Secondary Problems

### Commands

`systemctl --failed`

`journalctl -p err -b`

`df -h`

`free -h`

`ip -s link`

### Purpose

Confirm that the corrective action did not create another problem.

---

## 16. Step 15 — Document the Incident

### Record

- Incident ID
- Date / Time
- Affected Server
- Symptoms
- User Impact
- Initial Condition
- Evidence
- Investigation
- Root Cause
- Corrective Action
- Recovery
- Verification
- Final Result
- Lessons Learned

### Example

**Incident:** Application unavailable

**Symptoms:** Application connection failed

**Investigation:** Service status, logs, port, filesystem

**Root Cause:** Filesystem reached critical capacity

**Corrective Action:** Removed confirmed unnecessary data

**Recovery:** Application service restarted

**Verification:** Application responded successfully

**Result:** Service restored

---

## 17. End-to-End Troubleshooting Method

### Administer

Check and manage the Linux server.

↓

### Monitor

Check CPU, memory, disk, network, and services.

↓

### Troubleshoot

Investigate the reported symptoms.

↓

### Recover

Apply a controlled corrective action.

↓

### Verify

Confirm that the original problem is resolved.

↓

### Document

Record the incident, root cause, recovery, and lessons learned.

---

## 18. Complete Troubleshooting Command Set

### System

`hostname`

`date`

`uptime`

`who`

`w`

### Performance

`top`

`free -h`

`vmstat 1 5`

`iostat -xz 1 5`

### Processes

`ps aux --sort=-%cpu | head`

`ps aux --sort=-%mem | head`

`pgrep -a <process-name>`

### Services

`systemctl --failed`

`systemctl status <service>`

### Logs

`journalctl -p err -b`

`journalctl -u <service> -b`

### Network

`ip -br addr`

`ip route`

`ping <gateway-ip>`

`resolvectl query google.com`

### Ports

`sudo ss -tulpn`

`nc -zv <IP> <port>`

### Storage

`lsblk`

`lsblk -f`

`df -h`

`df -i`

`findmnt`

`sudo lsof +L1`

---

## 19. Real-Work End-to-End Workflow

Incident Report  
↓  
Confirm Impact  
↓  
Identify Server  
↓  
Collect Initial Evidence  
↓  
Check System Health  
↓  
Check Processes  
↓  
Check Services  
↓  
Investigate Logs  
↓  
Check Network  
↓  
Check Ports  
↓  
Check Storage  
↓  
Identify Root Cause  
↓  
Contain Incident  
↓  
Apply Corrective Action  
↓  
Recover Service  
↓  
Verify System  
↓  
Verify Application  
↓  
Check For Secondary Problems  
↓  
Document Incident  
↓  
Lessons Learned  
↓  
Close Incident

---

## 20. Final Project Outcome

This module demonstrates the complete Linux Server Operations & Troubleshooting workflow:

**Administer → Monitor → Troubleshoot → Recover → Verify → Document**

The completed project demonstrates practical ability to:

- Administer Linux servers.
- Manage users, groups, permissions, and services.
- Monitor system resources.
- Investigate logs and system behaviour.
- Troubleshoot network problems.
- Troubleshoot storage problems.
- Troubleshoot services and applications.
- Perform remote administration.
- Handle operational incidents.
- Identify root causes.
- Recover affected services.
- Verify successful recovery.
- Document technical incidents professionally.

## Project Status

**M01–M20: COMPLETED** ✅

**Project 1 — Linux Server Operations & Troubleshooting: COMPLETED** ✅

## Portfolio Outcome

This project demonstrates practical Linux Server administration and troubleshooting capability through a hands-on virtualised home-lab environment.

It provides evidence of the ability to:

**Administer → Monitor → Troubleshoot → Recover → Verify → Document**
