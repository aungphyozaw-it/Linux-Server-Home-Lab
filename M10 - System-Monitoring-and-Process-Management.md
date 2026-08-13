# M10 — System Monitoring & Process Management

## Objective

Understand how to monitor Linux system resources, processes, services, CPU, memory, load average, disk I/O, uptime, and system performance.

The goal is to develop practical monitoring and troubleshooting skills used in real Linux Server and Data Center Operations.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab
- Ubuntu Server VM

---

## 1. System Uptime

### Commands

`uptime`  
`w`  
`who -b`

### Key Points

- `uptime` — Shows current time, system uptime, logged-in users, and load average.
- `w` — Shows logged-in users and what they are doing.
- `who -b` — Shows the last system boot time.

---

## 2. CPU Information

### Commands

`lscpu`  
`nproc`  
`cat /proc/cpuinfo`

### Key Points

- `lscpu` — Displays CPU architecture and processor information.
- `nproc` — Shows the number of available CPU processing units.
- `/proc/cpuinfo` — Provides detailed CPU information.

---

## 3. Memory Monitoring

### Commands

`free -h`  
`free -m`  
`cat /proc/meminfo`

### Key Points

- `free -h` — Displays RAM and swap usage in human-readable format.
- `free -m` — Displays memory usage in MB.
- `/proc/meminfo` — Provides detailed memory information.

### Important Areas

- Total memory
- Used memory
- Available memory
- Free memory
- Buffers
- Cache
- Swap

---

## 4. Process Monitoring — ps

### Commands

`ps`  
`ps aux`  
`ps -ef`  
`ps -u $USER`  
`ps aux --sort=-%cpu`  
`ps aux --sort=-%mem`

### Key Points

- `ps aux` — Displays running processes for all users.
- `ps -ef` — Displays processes in full format.
- `ps -u $USER` — Shows processes belonging to the current user.
- `--sort=-%cpu` — Sorts processes by CPU usage.
- `--sort=-%mem` — Sorts processes by memory usage.

---

## 5. Process Monitoring — top

### Command

`top`

### Useful Interactive Keys

`P` — Sort by CPU usage  
`M` — Sort by memory usage  
`k` — Kill a process  
`r` — Change process priority  
`1` — Show individual CPU cores  
`q` — Quit

### Key Information

`top` displays:

- CPU usage
- Memory usage
- Load average
- Running processes
- Process IDs
- CPU percentage
- Memory percentage
- Process state

---

## 6. Process Monitoring — htop

### Install

`sudo apt update`  
`sudo apt install htop`

### Command

`htop`

### Key Point

`htop` provides an interactive and easier-to-read process monitoring interface.

---

## 7. Process Identification

### Commands

`pgrep <process-name>`  
`pidof <process-name>`  
`ps aux | grep <process-name>`

### Example

`pgrep ssh`  
`pidof sshd`  
`ps aux | grep ssh`

### Key Point

These commands help identify the PID of a running process.

---

## 8. Process Details

### Commands

`ps -p <PID> -f`  
`cat /proc/<PID>/status`  
`ls -l /proc/<PID>/exe`  
`ls -l /proc/<PID>/cwd`

### Useful Information

- Process owner
- Parent process
- Process state
- Memory usage
- Executable path
- Current working directory

---

## 9. Process Tree

### Commands

`pstree`  
`pstree -p`  
`ps --forest`

### Key Point

Process trees help identify parent-child relationships between processes.

---

## 10. Process States

### Common States

`R` — Running  
`S` — Sleeping  
`D` — Uninterruptible sleep  
`T` — Stopped  
`Z` — Zombie

### Check

`ps aux`

### Key Point

A large number of `Z` zombie processes can indicate a problem with parent processes not properly collecting child process exit statuses.

---

## 11. Foreground and Background Processes

### Commands

`command &`  
`jobs`  
`fg`  
`bg`

### Key Points

- `&` — Run a command in the background.
- `jobs` — Display jobs started from the current shell.
- `fg` — Bring a background job to the foreground.
- `bg` — Continue a stopped job in the background.

---

## 12. Running Process in Background

### Example

`ping 8.8.8.8 &`

### Check

`jobs`

### Bring to Foreground

`fg`

### Key Point

Background execution is useful for long-running commands and administrative tasks.

---

## 13. Process Termination

### Commands

`kill <PID>`  
`kill -15 <PID>`  
`kill -9 <PID>`  
`pkill <process-name>`  
`killall <process-name>`

### Signals

`SIGTERM (15)` — Gracefully request process termination.

`SIGKILL (9)` — Forcefully terminate a process.

### Best Practice

Try:

`kill <PID>`

or:

`kill -15 <PID>`

before using:

`kill -9 <PID>`

### Important Warning

Do not use `kill -9` as the first option unless necessary.

---

## 14. CPU Load Average

### Commands

`uptime`  
`top`  
`cat /proc/loadavg`

### Load Average

Linux displays three values:

- 1-minute load average
- 5-minute load average
- 15-minute load average

### Important Concept

Load average represents the amount of work waiting for or using CPU and other scheduling resources.

Compare load average with the number of CPU cores before deciding whether the system is overloaded.

Check CPU count with:

`nproc`

---

## 15. CPU Usage Troubleshooting

### Commands

`top`  
`ps aux --sort=-%cpu | head`  
`uptime`  
`lscpu`

### Troubleshooting Flow

1. Check load average.
2. Check CPU count.
3. Identify high-CPU processes.
4. Check whether the process is expected.
5. Check logs.
6. Investigate the application.
7. Stop or restart the process only when appropriate.
8. Verify CPU usage again.

---

## 16. Memory Usage Troubleshooting

### Commands

`free -h`  
`top`  
`ps aux --sort=-%mem | head`  
`cat /proc/meminfo`

### Troubleshooting Flow

1. Check available memory.
2. Check swap usage.
3. Identify processes consuming memory.
4. Check whether memory usage is expected.
5. Investigate application behaviour.
6. Check logs.
7. Take corrective action.
8. Verify memory usage again.

---

## 17. Swap Monitoring

### Commands

`free -h`  
`swapon --show`  
`cat /proc/swaps`

### Key Points

Monitor:

- Total swap
- Used swap
- Available swap
- Active swap devices/files

Heavy swap usage can indicate memory pressure.

---

## 18. System Load and Resource Overview

### Commands

`uptime`  
`top`  
`free -h`  
`df -h`  
`vmstat`

### Key Point

These commands provide a quick overview of system health.

---

## 19. vmstat — Virtual Memory Statistics

### Commands

`vmstat`  
`vmstat 1`  
`vmstat 1 10`

### Key Areas

- Processes
- Memory
- Swap
- I/O
- System activity
- CPU

### Example

`vmstat 1`

Updates system statistics every second.

---

## 20. Disk Space Monitoring

### Commands

`df -h`  
`df -Th`  
`du -sh *`

### Key Point

Use `df` to check filesystem capacity and `du` to identify where space is being consumed.

---

## 21. Disk I/O Monitoring

### Commands

`iostat`  
`vmstat 1`  
`dmesg | grep -i "I/O"`

### Install iostat

`sudo apt update`  
`sudo apt install sysstat`

### Run

`iostat`  
`iostat -xz 1`

### Key Information

- Read operations
- Write operations
- I/O wait
- Device utilization
- Throughput

---

## 22. I/O Wait Troubleshooting

### Commands

`top`  
`iostat -xz 1`  
`vmstat 1`  
`dmesg | grep -i error`

### Possible Causes

- Slow storage
- Disk failure
- High disk activity
- Storage controller problems
- Application performing heavy I/O

---

## 23. Network Monitoring Basics

### Commands

`ip -s link`  
`ss -s`  
`ss -tuln`  
`ping 8.8.8.8`

### Key Points

- `ip -s link` — Shows network interface statistics.
- `ss -s` — Displays socket statistics.
- `ss -tuln` — Displays listening TCP/UDP sockets.
- `ping` — Tests basic network connectivity.

---

## 24. Network Interface Errors

### Command

`ip -s link`

### Check

- RX errors
- TX errors
- Dropped packets
- Packet counts

### Troubleshooting

`ip -s link`  
`dmesg | grep -i network`  
`sudo journalctl -k`

---

## 25. Service Monitoring

### Commands

`systemctl status <service>`  
`systemctl is-active <service>`  
`systemctl is-enabled <service>`  
`systemctl list-units --type=service`

### Example

`systemctl status ssh`  
`systemctl is-active ssh`  
`systemctl is-enabled ssh`

### Key Point

These commands determine whether a service is running and whether it is configured to start automatically.

---

## 26. Service Resource Investigation

### Commands

`systemctl status <service>`  
`systemctl show <service>`  
`systemctl cat <service>`

### Key Point

Useful when investigating abnormal service behaviour.

---

## 27. System Logs

### Commands

`journalctl`  
`journalctl -b`  
`journalctl -p err`  
`journalctl -p warning`  
`journalctl -xe`

### Key Points

- `journalctl` — View systemd journal logs.
- `journalctl -b` — Logs from the current boot.
- `journalctl -p err` — Error-level messages.
- `journalctl -p warning` — Warning-level messages.
- `journalctl -xe` — Recent detailed system messages.

---

## 28. Service Logs

### Commands

`journalctl -u <service>`  
`journalctl -u <service> -b`  
`journalctl -u <service> --since "1 hour ago"`

### Example

`journalctl -u ssh`  
`journalctl -u ssh -b`

### Key Point

Useful for troubleshooting failed or unstable services.

---

## 29. Real-Time Log Monitoring

### Command

`journalctl -f`

### Service-Specific

`journalctl -u ssh -f`

### Key Point

`-f` follows new log entries in real time.

---

## 30. Process Resource Limits

### Commands

`ulimit -a`  
`ulimit -n`  
`cat /proc/<PID>/limits`

### Key Point

Resource limits can affect applications when they reach limits such as:

- Open files
- Processes
- Memory
- CPU time

---

## 31. Open Files

### Commands

`lsof`  
`sudo lsof -i`  
`sudo lsof -p <PID>`  
`sudo lsof /path/to/file`

### Key Point

`lsof` means List Open Files.

It is extremely useful for real-world troubleshooting.

---

## 32. Network Connections by Process

### Commands

`sudo lsof -i`  
`sudo lsof -i :22`  
`ss -tulpn`

### Example

`sudo lsof -i :22`

Useful for identifying which process is using a network port.

---

## 33. Check Listening Ports

### Command

`ss -tuln`

### With Process Information

`sudo ss -tulpn`

### Key Points

Identify:

- Listening TCP ports
- Listening UDP ports
- Local addresses
- Port numbers
- Associated processes

---

## 34. Process Priority

### Commands

`nice`  
`renice`

### Check Priority

`ps -eo pid,ni,pri,comm`

### Start With Priority

`nice -n 10 command`

### Change Priority

`sudo renice 10 -p <PID>`

### Key Point

Nice values influence process CPU scheduling priority.

---

## 35. System Performance Snapshot

### Commands

`uptime`  
`free -h`  
`df -h`  
`vmstat 1 5`  
`iostat -xz 1 5`  
`ps aux --sort=-%cpu | head`  
`ps aux --sort=-%mem | head`

### Purpose

Use these commands together when performing a quick system health check.

---

## 36. High CPU Troubleshooting Scenario

### Scenario

A Linux server is running slowly and users report application delays.

### Investigation

`uptime`

Check load average.

`nproc`

Check CPU count.

`top`

Identify high CPU processes.

`ps aux --sort=-%cpu | head`

Identify the top CPU-consuming processes.

### Check Logs

`journalctl -p err`

### Action

Investigate the responsible process or application before terminating it.

### Verification

`top`  
`uptime`

Confirm that CPU and load conditions have improved.

---

## 37. High Memory Troubleshooting Scenario

### Scenario

An application becomes slow and the server starts using large amounts of swap.

### Investigation

`free -h`

Check memory and swap.

`ps aux --sort=-%mem | head`

Identify high-memory processes.

`top`

Monitor the process in real time.

### Check Logs

`journalctl -p err`

### Verification

`free -h`  
`swapon --show`

---

## 38. Disk I/O Troubleshooting Scenario

### Scenario

Applications are slow even though CPU usage is normal.

### Investigation

`iostat -xz 1`

Check disk utilization and I/O wait.

`vmstat 1`

Check CPU and I/O activity.

`dmesg | grep -i error`

Check for storage errors.

### Verification

`iostat -xz 1`

Confirm whether I/O performance has improved.

---

## 39. Service Failure Troubleshooting Scenario

### Scenario

SSH or another server service stops working.

### Investigation

`systemctl status ssh`

Check service state.

`systemctl is-active ssh`

Check whether the service is active.

`journalctl -u ssh -b`

Check service logs.

`ss -tulpn`

Check whether the expected port is listening.

### Verification

`systemctl status ssh`  
`ss -tulpn`

---

## 40. Process Troubleshooting Workflow

### Real-Work Workflow

1. Confirm the problem.
2. Check system uptime and load.
3. Check CPU usage.
4. Check memory and swap.
5. Check disk space.
6. Check disk I/O.
7. Check network statistics.
8. Identify problematic processes.
9. Check service status.
10. Check system logs.
11. Apply the appropriate fix.
12. Verify system health.

### Core Commands

`uptime`  
`top`  
`ps aux`  
`free -h`  
`vmstat`  
`iostat`  
`df -h`  
`ss -tuln`  
`systemctl status`  
`journalctl`  
`lsof`  
`dmesg`

---

## 41. Monitoring Checklist

### CPU

`uptime`  
`top`  
`lscpu`  
`ps aux --sort=-%cpu | head`

### Memory

`free -h`  
`top`  
`ps aux --sort=-%mem | head`

### Disk

`df -h`  
`du -sh *`  
`iostat -xz 1`

### Network

`ip -s link`  
`ss -s`  
`ss -tuln`

### Services

`systemctl status <service>`  
`systemctl is-active <service>`  
`systemctl is-enabled <service>`

### Logs

`journalctl -p err`  
`journalctl -u <service>`  
`dmesg | tail -50`

---

## 42. Important Monitoring Commands Summary

### System

`uptime`  
`w`  
`who -b`

### CPU

`lscpu`  
`nproc`  
`top`

### Memory

`free -h`  
`vmstat`  
`cat /proc/meminfo`

### Processes

`ps aux`  
`ps -ef`  
`pgrep`  
`pidof`  
`pstree`  
`top`

### Disk

`df -h`  
`du -sh *`  
`iostat`

### Network

`ip -s link`  
`ss -s`  
`ss -tuln`

### Services

`systemctl status`  
`systemctl is-active`  
`systemctl is-enabled`

### Logs

`journalctl`  
`journalctl -u`  
`dmesg`

### Open Files

`lsof`

---

## Result

Successfully studied Linux system monitoring and process management.

The module covered:

- System uptime
- CPU information
- Memory monitoring
- Process monitoring
- `ps`
- `top`
- `htop`
- Process identification
- Process states
- Foreground and background processes
- Process termination
- CPU load average
- CPU troubleshooting
- Memory troubleshooting
- Swap monitoring
- `vmstat`
- Disk monitoring
- Disk I/O
- Network monitoring
- Service monitoring
- System logs
- Service logs
- Real-time log monitoring
- Resource limits
- Open files
- Listening ports
- Process priority
- Real-world troubleshooting scenarios

## Module Status

**M10 — COMPLETED** ✅
