 # M12 — Process Management & Troubleshooting

## Objective

Learn how to identify, monitor, control, and troubleshoot Linux processes in real-world server operations.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. View Running Processes

### Commands

`ps`  
`ps aux`  
`ps -ef`

### Key Points

- `ps aux` — View processes from all users.
- `ps -ef` — View processes in full format.
- Use these commands to identify process IDs and process ownership.

---

## 2. Find a Specific Process

### Commands

`pgrep <process-name>`  
`pidof <process-name>`  
`ps aux | grep <process-name>`

### Example

`pgrep ssh`  
`pidof sshd`

### Key Point

The main information needed is usually the **PID (Process ID)**.

---

## 3. Process Details

### Commands

`ps -p <PID> -f`  
`cat /proc/<PID>/status`  
`ls -l /proc/<PID>/exe`

### Check

- PID
- Parent process
- User
- Process state
- Executable
- Memory information

---

## 4. Process Tree

### Commands

`pstree`  
`pstree -p`  
`ps --forest`

### Key Point

Useful for understanding parent-child process relationships.

---

## 5. Process States

### Common States

`R` — Running  
`S` — Sleeping  
`D` — Uninterruptible sleep  
`T` — Stopped  
`Z` — Zombie

### Check

`ps aux`

### Key Point

A large number of zombie processes may indicate a problem with parent processes.

---

## 6. Real-Time Process Monitoring

### Command

`top`

### Important Keys

`P` — Sort by CPU usage  
`M` — Sort by memory usage  
`k` — Kill a process  
`r` — Change process priority  
`q` — Quit

### Key Point

Use `top` to identify processes consuming excessive resources.

---

## 7. Interactive Process Monitoring

### Install

`sudo apt update`  
`sudo apt install htop`

### Command

`htop`

### Key Point

`htop` provides an interactive view of processes and their resource usage.

---

## 8. Foreground & Background Processes

### Commands

`command &`  
`jobs`  
`fg`  
`bg`

### Example

`ping 8.8.8.8 &`

### Check

`jobs`

### Key Point

Useful when running long-running commands without blocking the current shell.

---

## 9. Stop a Process

### Commands

`kill <PID>`  
`kill -15 <PID>`  
`kill -9 <PID>`

### Best Practice

Try:

`kill <PID>`

or:

`kill -15 <PID>`

before using:

`kill -9 <PID>`

### Key Points

`SIGTERM (15)` — Gracefully requests termination.

`SIGKILL (9)` — Forces termination.

### Warning

Do not use `kill -9` as the first option unless necessary.

---

## 10. Kill Processes by Name

### Commands

`pkill <process-name>`  
`killall <process-name>`

### Warning

Be careful when terminating processes by name because multiple processes may be affected.

---

## 11. Process Priority

### Commands

`nice -n 10 <command>`  
`renice 10 -p <PID>`  
`ps -eo pid,ni,pri,comm`

### Key Point

Nice values influence CPU scheduling priority.

Use priority changes carefully on production systems.

---

## 12. Find High-CPU Processes

### Commands

`ps aux --sort=-%cpu | head`  
`top`

### Troubleshooting

1. Identify the process.
2. Check the PID.
3. Identify the application or service.
4. Check whether the high usage is expected.
5. Investigate logs and configuration.
6. Take corrective action.
7. Verify CPU usage.

---

## 13. Find High-Memory Processes

### Commands

`ps aux --sort=-%mem | head`  
`top`

### Troubleshooting

1. Identify the process.
2. Check memory usage.
3. Determine whether the usage is expected.
4. Check for abnormal growth.
5. Investigate the application.
6. Take corrective action.
7. Verify memory usage.

---

## 14. Zombie Process Troubleshooting

### Check

`ps aux | grep Z`

### More Detailed

`ps -eo pid,ppid,state,cmd | grep Z`

### Investigation

Identify:

- Zombie PID
- Parent PID
- Parent process

### Key Point

A zombie process has already exited but its parent has not collected its exit status.

Do not simply use `kill -9` on a zombie because the zombie itself is already terminated.

Investigate the parent process instead.

---

## 15. Unresponsive Process

### Scenario

An application process becomes unresponsive.

### Step 1 — Identify

`pgrep <process-name>`

### Step 2 — Inspect

`ps -p <PID> -f`

### Step 3 — Monitor

`top`

### Step 4 — Try Graceful Termination

`kill -15 <PID>`

### Step 5 — Verify

`ps -p <PID>`

### Step 6 — Force Termination Only If Necessary

`kill -9 <PID>`

### Step 7 — Verify

`ps -p <PID>`

---

## 16. Process and Service Relationship

### Commands

`systemctl status <service>`  
`ps -ef | grep <process-name>`  
`pgrep -a <process-name>`

### Key Point

A systemd service normally manages one or more processes.

When troubleshooting, identify both:

- The service
- The underlying process

---

## 17. Open Files Used by a Process

### Commands

`sudo lsof -p <PID>`  
`sudo lsof -i -a -p <PID>`

### Key Point

Useful for identifying files, sockets, and network connections associated with a process.

---

## 18. Process Troubleshooting Scenario

### Scenario

A server application is consuming excessive CPU.

### Investigation

`ps aux --sort=-%cpu | head`

Identify the PID.

`ps -p <PID> -f`

Check process details.

`top`

Monitor the process.

`systemctl status <service>`

Check the related service if applicable.

`journalctl -u <service> -b`

Check service logs.

### Action

Determine the root cause before restarting or terminating the process.

### Verification

`top`  
`ps -p <PID>`

Confirm that the problem has been resolved.

---

## 19. Process Troubleshooting Scenario — Memory Leak

### Scenario

An application continuously consumes more memory.

### Investigation

`ps aux --sort=-%mem | head`

`top`

`ps -p <PID> -f`

### Check Logs

`journalctl -u <service> -b`

### Action

Investigate the application/service rather than repeatedly killing the process without finding the root cause.

### Verification

`free -h`  
`top`

---

## 20. Important Commands Summary

### Process Identification

`ps aux`  
`ps -ef`  
`pgrep <process-name>`  
`pidof <process-name>`  
`pstree -p`

### Process Monitoring

`top`  
`htop`

### Process Control

`kill <PID>`  
`kill -15 <PID>`  
`kill -9 <PID>`  
`pkill <process-name>`  
`killall <process-name>`

### Background Jobs

`jobs`  
`fg`  
`bg`

### Priority

`nice`  
`renice`

### Investigation

`ps -p <PID> -f`  
`cat /proc/<PID>/status`  
`sudo lsof -p <PID>`

### Process Troubleshooting

`ps aux --sort=-%cpu | head`  
`ps aux --sort=-%mem | head`

---

## Real-Work Process Troubleshooting Flow

Process Problem  
↓  
Identify Process  
↓  
`pgrep <process-name>`  
↓  
Get PID  
↓  
`ps -p <PID> -f`  
↓  
Monitor  
↓  
`top`  
↓  
Check Service  
↓  
`systemctl status <service>`  
↓  
Check Logs  
↓  
`journalctl -u <service> -b`  
↓  
Apply Controlled Fix  
↓  
Verify Process  
↓  
Verify Service / Application

---

## Result

Successfully studied the main Linux process-management and troubleshooting techniques required for server operations.

The module covered:

- Process identification
- PID management
- Process details
- Process trees
- Process states
- `ps`
- `top`
- `htop`
- Foreground/background processes
- Process termination
- Process priority
- High-CPU troubleshooting
- High-memory troubleshooting
- Zombie processes
- Unresponsive processes
- Process/service relationship
- Open files and sockets
- Process troubleshooting workflow

## Module Status

**M12 — COMPLETED** ✅
