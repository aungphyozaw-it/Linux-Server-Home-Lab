# M16 — Service & Application Troubleshooting

## Objective

Learn how to troubleshoot Linux services and applications by checking service status, ports, logs, configuration, dependencies, and application behaviour.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Identify the Problem

### Check Service

`systemctl status <service>`

### Check Process

`pgrep -a <process-name>`

### Check Port

`sudo ss -tulpn`

### Key Point

First identify whether the problem is related to:

- Service
- Process
- Port
- Configuration
- Dependency
- Application
- Network connectivity

---

## 2. Check Service Status

### Commands

`systemctl is-active <service>`  
`systemctl is-enabled <service>`  
`systemctl status <service>`

### Key Point

Determine whether the service is:

- Running
- Stopped
- Failed
- Disabled
- Restarting

---

## 3. Check Service Logs

### Commands

`journalctl -u <service> -b`  
`journalctl -u <service> --since "1 hour ago"`  
`journalctl -u <service> -f`

### Key Point

Logs are one of the most important sources of evidence when troubleshooting service failures.

---

## 4. Check Listening Ports

### Commands

`sudo ss -tulpn`  
`sudo ss -tulpn | grep :22`

### Check

- Expected port
- Listening address
- Protocol
- Process using the port

### Key Point

A service may be running but not listening on the expected port.

---

## 5. Test Application Port

### Commands

`nc -zv <server-ip> <port>`  
`curl -I http://<server-ip>:<port>`

### Key Point

Use a local or remote connection test to determine whether the application is actually reachable.

---

## 6. Check Configuration

### Commands

`systemctl cat <service>`  
`systemctl show <service>`  
`sudo <application> --help`

### Key Point

Check configuration paths, startup options, environment variables, and service parameters before changing anything.

---

## 7. Validate Configuration

### Examples

`sudo sshd -t`

For other applications, use the application's own configuration-test command when available.

### Key Point

Always validate configuration before restarting a production service.

---

## 8. Check Dependencies

### Commands

`systemctl list-dependencies <service>`  
`systemctl list-dependencies --reverse <service>`

### Key Point

A service may fail because another required service or resource is unavailable.

---

## 9. Check Recent System Errors

### Commands

`journalctl -p err -b`  
`systemctl --failed`

### Key Point

Useful when the application failure may be related to another system component.

---

## 10. Restart and Verify

### Command

`sudo systemctl restart <service>`

### Verify

`systemctl status <service>`  
`systemctl is-active <service>`  
`sudo ss -tulpn | grep :<port>`

### Key Point

Never consider a restart successful until the service and application have been verified.

---

## 11. Application Not Starting

### Scenario

An application fails to start.

### Investigation

`systemctl status <service>`

`journalctl -u <service> -b`

`systemctl --failed`

`systemctl list-dependencies <service>`

### Check Configuration

Use the application's configuration-test command if available.

### Check Port

`sudo ss -tulpn`

### Action

Identify the root cause before restarting repeatedly.

### Verification

`systemctl is-active <service>`

---

## 12. Service Running but Application Unreachable

### Scenario

The service shows `active`, but users cannot connect.

### Check Process

`pgrep -a <process-name>`

### Check Port

`sudo ss -tulpn | grep :<port>`

### Test Locally

`nc -zv 127.0.0.1 <port>`

### Test Application

`curl -I http://127.0.0.1:<port>`

### Check Firewall

`sudo ufw status`

### Check Logs

`journalctl -u <service> -b`

### Key Point

A running service does not automatically mean the application is functioning correctly.

---

## 13. Service Keeps Restarting

### Check Status

`systemctl status <service>`

### Check Logs

`journalctl -u <service> -b`

### Check Process

`pgrep -a <process-name>`

### Check Configuration

Use the application's configuration-test command.

### Check Dependencies

`systemctl list-dependencies <service>`

### Key Point

Repeatedly restarting the service without finding the root cause can hide the actual problem.

---

## 14. Port Conflict

### Scenario

An application cannot start because its port is already in use.

### Check Port

`sudo ss -tulpn | grep :<port>`

### Identify Process

`sudo lsof -i :<port>`

### Check Process

`ps -p <PID> -f`

### Action

Determine which application owns the port before stopping anything.

### Verification

`sudo ss -tulpn | grep :<port>`

---

## 15. Permission Problem

### Check File

`ls -l <file>`

### Check Directory

`ls -ld <directory>`

### Check Ownership

`stat <file>`

### Check Service User

`systemctl show <service> -p User`

### Key Point

Applications may fail because their service account cannot access required files or directories.

---

## 16. Disk Space Affecting an Application

### Commands

`df -h`  
`du -sh <directory>`

### Check Logs

`journalctl -u <service> -b`

### Key Point

A full filesystem can cause applications to fail when they cannot create logs, temporary files, databases, or other required data.

---

## 17. Application Troubleshooting Workflow

### Step 1 — Confirm the Problem

Identify what users or monitoring systems are reporting.

### Step 2 — Check Service

`systemctl status <service>`

### Step 3 — Check Logs

`journalctl -u <service> -b`

### Step 4 — Check Process

`pgrep -a <process-name>`

### Step 5 — Check Port

`sudo ss -tulpn | grep :<port>`

### Step 6 — Check Configuration

Validate the application's configuration.

### Step 7 — Check Dependencies

`systemctl list-dependencies <service>`

### Step 8 — Check Resources

`df -h`  
`free -h`

### Step 9 — Apply Controlled Fix

Make the smallest appropriate change.

### Step 10 — Verify

Confirm:

- Service is active.
- Process is running.
- Port is listening.
- Application responds.
- Logs show no new errors.

---

## 18. Important Commands Summary

### Service

`systemctl status <service>`  
`systemctl is-active <service>`  
`systemctl restart <service>`

### Logs

`journalctl -u <service> -b`  
`journalctl -u <service> -f`

### Process

`pgrep -a <process-name>`  
`ps -p <PID> -f`

### Ports

`sudo ss -tulpn`  
`sudo lsof -i :<port>`  
`nc -zv <IP> <port>`

### Application Testing

`curl -I http://<server-ip>:<port>`

### Dependencies

`systemctl list-dependencies <service>`

### System Errors

`journalctl -p err -b`  
`systemctl --failed`

### Resources

`df -h`  
`free -h`

---

## Real-Work Troubleshooting Flow

Application Problem  
↓  
Confirm the problem  
↓  
`systemctl status <service>`  
↓  
`journalctl -u <service> -b`  
↓  
Check process  
↓  
`pgrep -a <process-name>`  
↓  
Check port  
↓  
`sudo ss -tulpn`  
↓  
Check configuration / dependencies  
↓  
Check disk / memory  
↓  
Identify Root Cause  
↓  
Apply Controlled Fix  
↓  
Restart if required  
↓  
Verify Service  
↓  
Verify Port  
↓  
Verify Application  
↓  
Document Result

---

## Result

Successfully studied the main service and application troubleshooting techniques required for Linux server operations.

The module covered:

- Service failure investigation
- Application startup problems
- Service logs
- Process verification
- Port troubleshooting
- Configuration validation
- Service dependencies
- Port conflicts
- Permission problems
- Disk-space-related application failures
- Application connectivity testing
- Structured troubleshooting workflow

## Module Status

**M16 — COMPLETED** ✅
