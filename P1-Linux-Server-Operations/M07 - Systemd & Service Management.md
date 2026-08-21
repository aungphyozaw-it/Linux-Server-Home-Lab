# M07 — Systemd & Service Management
 
## Objective
 
Understand systemd and systemctl, manage Linux services, work with system targets, and troubleshoot service-related problems on Ubuntu Server.
 
## Lab Environment
 
 
- Ubuntu Server
 
- Oracle VirtualBox
 
- Linux Server Home Lab
 

  
# 1. systemd Fundamentals
 
`systemd` is the system and service manager used by modern Ubuntu systems.
 
It manages:
 
 
- Services
 
- System targets
 
- Boot process
 
- Background processes
 
- Service dependencies
 

 
### Check systemd Version
 `systemd --version`  
### Check systemd as PID 1
 `ps -p 1 -o pid,comm,args`  
**Purpose:** Verify that systemd is running as the system's initial process.
  
# 2. systemctl Basics
 
`systemctl` is the primary command used to interact with systemd.
 
### Display systemctl Help
 `systemctl --help`  
### Check systemd Manager Status
 `systemctl status`  
**Purpose:** Display the overall systemd manager status.
  
# 3. Service Status
 
### Check Service Status
 `systemctl status <service>`  
Example:
 `systemctl status ssh`  
### Check Whether a Service Is Active
 `systemctl is-active <service>`  
### Check Whether a Service Is Enabled
 `systemctl is-enabled <service>`  
### Purpose
 
Use these commands to determine:
 
 
- Whether a service is running
 
- Whether a service starts automatically at boot
 
- Whether a service has failed
 

  
# 4. Start a Service
 
### Command
 `sudo systemctl start <service>`  
Example:
 `sudo systemctl start ssh`  
**Purpose:** Start a stopped service.
  
# 5. Stop a Service
 
### Command
 `sudo systemctl stop <service>`  
Example:
 `sudo systemctl stop ssh`  
**Purpose:** Stop a running service.
  
# 6. Restart a Service
 
### Command
 `sudo systemctl restart <service>`  
Example:
 `sudo systemctl restart ssh`  
**Purpose:** Stop and start a service again.
 
Commonly used after changing service configuration.
  
# 7. Reload a Service
 
### Command
 `sudo systemctl reload <service>`  
**Purpose:** Reload a service's configuration without completely restarting the service, when the service supports reload.
 
### Restart vs Reload
 `restart   ↓ Stop + Start  reload   ↓ Reload Configuration`   
# 8. Enable / Disable Services
 
### Enable Service
 `sudo systemctl enable <service>`  
**Purpose:** Configure the service to start automatically during boot.
 
### Disable Service
 `sudo systemctl disable <service>`  
**Purpose:** Prevent the service from automatically starting during boot.
 
### Enable and Start Immediately
 `sudo systemctl enable --now <service>`  
**Purpose:** Enable the service for boot and start it immediately.
 
### Disable and Stop
 `sudo systemctl disable --now <service>`  
**Purpose:** Disable the service and stop it immediately.
  
# 9. List Running Services
 
### List Active Services
 `systemctl list-units --type=service`  
**Purpose:** Display currently loaded service units.
 
### List All Service Units
 `systemctl list-units --type=service --all`  
**Purpose:** Display active and inactive service units.
 
### Lab Practice
 
`systemctl list-units` was used during the Linux lab to inspect system units.
  
# 10. Failed Services
 
### List Failed Units
 `systemctl --failed`  
### List Failed Service Units
 `systemctl list-units --failed --type=service`  
**Purpose:** Quickly identify services that have entered a failed state.
  
# 11. Service Logs
 
systemd uses the journal to record service and system events.
 
### View Service Logs
 `sudo journalctl -u <service>`  
Example:
 `sudo journalctl -u ssh`  
### View Recent Service Logs
 `sudo journalctl -u <service> -n 50`  
### Follow Service Logs Live
 `sudo journalctl -u <service> -f`  
**Purpose:** Investigate why a service is failing or behaving unexpectedly.
  
# 12. Service Unit Information
 
### Show Service Definition
 `systemctl cat <service>`  
Example:
 `systemctl cat ssh`  
**Purpose:** Display the service unit configuration.
 
### Show Service Properties
 `systemctl show <service>`  
**Purpose:** Display detailed systemd properties for a service.
  
# 13. System Targets
 
A systemd target represents a system state or group of units.
 
### Check Default Target
 `systemctl get-default`  
### List Available Targets
 `systemctl list-units --type=target`  
### Set Default Target
 `sudo systemctl set-default <target>`  
### Switch Target
 `sudo systemctl isolate <target>`  
### Lab Practice
 
The lab included:
 `systemctl get-default`  
and practising system target changes.
  
# 14. Common System Targets
 
Important targets to understand:
 `multi-user.target graphical.target rescue.target emergency.target`  
### Purpose
 
 
- `multi-user.target` — Multi-user command-line environment.
 
- `graphical.target` — Graphical desktop environment.
 
- `rescue.target` — Rescue/maintenance environment.
 
- `emergency.target` — Minimal emergency environment.
 

  
# 15. Service Troubleshooting
 
When a service is not working, follow a structured process.
 
### Step 1 — Check Status
 `systemctl status <service>`  
### Step 2 — Check Whether It Is Active
 `systemctl is-active <service>`  
### Step 3 — Check Failed Units
 `systemctl --failed`  
### Step 4 — Check Logs
 `sudo journalctl -u <service>`  
### Step 5 — Check Recent Logs
 `sudo journalctl -u <service> -n 50`  
### Step 6 — Validate Configuration
 
If the service has its own configuration validation command, use it before restarting.
 
### Step 7 — Restart Service
 `sudo systemctl restart <service>`  
### Step 8 — Verify
 `systemctl status <service>`   
# 16. Boot Troubleshooting
 
If a service fails after reboot, check whether it is enabled.
 
### Commands
 `systemctl is-enabled <service>`  `systemctl status <service>`  `sudo journalctl -b -u <service>`  
**Purpose:** Investigate service behaviour during the current boot.
  
# 17. systemd Boot Target Troubleshooting
 
### Commands
 `systemctl get-default`  `systemctl list-units --type=target`  `systemctl list-dependencies`  
**Purpose:** Understand the current system target and its dependencies.
  
# 18. Practical SSH Service Example
 
SSH is a good example of systemd service management.
 
### Check SSH
 `systemctl status ssh`  
### Start SSH
 `sudo systemctl start ssh`  
### Restart SSH
 `sudo systemctl restart ssh`  
### Enable SSH
 `sudo systemctl enable ssh`  
### Check SSH Logs
 `sudo journalctl -u ssh`   
# 19. Lab Practice
 
The lab included practical systemd/service-management tasks:
 
 
- Checking the default system target
 
- Changing system targets
 
- Listing systemd units
 
- Checking SSH service status
 
- Starting and managing services
 
- Troubleshooting service state
 

 
Example command used:
 `systemctl get-default`  
and:
 systemctl list-units   
# 20. Commands Summary
 
  
 
Title
 
Commands
 
   
 
systemd Fundamentals
 
`systemd --version`, `ps -p 1 -o pid,comm,args`
 
 
 
systemctl Basics
 
`systemctl --help`, `systemctl status`
 
 
 
Service Status
 
`systemctl status`, `systemctl is-active`, `systemctl is-enabled`
 
 
 
Start Service
 
`sudo systemctl start`
 
 
 
Stop Service
 
`sudo systemctl stop`
 
 
 
Restart Service
 
`sudo systemctl restart`
 
 
 
Reload Service
 
`sudo systemctl reload`
 
 
 
Enable / Disable
 
`sudo systemctl enable`, `disable`
 
 
 
Enable + Start
 
`sudo systemctl enable --now`
 
 
 
Running Services
 
`systemctl list-units --type=service`
 
 
 
All Services
 
`systemctl list-units --type=service --all`
 
 
 
Failed Services
 
`systemctl --failed`
 
 
 
Service Logs
 
`journalctl -u`
 
 
 
Service Unit
 
`systemctl cat`, `systemctl show`
 
 
 
Default Target
 
`systemctl get-default`
 
 
 
Targets
 
`systemctl list-units --type=target`
 
 
 
Change Target
 
`systemctl set-default`, `systemctl isolate`
 
 
 
Boot Logs
 
`journalctl -b -u`
 
  
  
# Result
 
Successfully practised systemd and service management on Ubuntu Server.
 
The lab included:
 
 
- systemd fundamentals
 
- `systemctl`
 
- Service status
 
- Service management
 
- System targets
 
- Default target
 
- Listing systemd units
 
- SSH service management
 
- Basic service troubleshooting
 
- Service log investigation
 

 
# Module Status
 
**M07 — COMPLETED** ✅
