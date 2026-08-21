# M17 — System Performance Troubleshooting

## Objective

Learn how to investigate and troubleshoot Linux server performance problems by identifying CPU, memory, disk I/O, and system resource bottlenecks.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Confirm the Performance Problem

### Commands

`uptime`  
`top`  
`free -h`  
`df -h`

### Check

- High CPU
- High memory usage
- High load average
- Low disk space
- Slow system response

### Key Point

First confirm that a performance problem actually exists before changing anything.

---

## 2. Check Load Average

### Commands

`uptime`  
`cat /proc/loadavg`  
`nproc`

### Key Point

Compare the load average with the number of available CPU cores.

A high load does not automatically mean CPU usage is the root cause. Investigate CPU, memory, and I/O together.

---

## 3. CPU Bottleneck Investigation

### Commands

`top`  
`ps aux --sort=-%cpu | head`

### Check

- CPU-consuming processes
- CPU utilisation
- System load
- Number of CPU cores

### Investigation

Identify the process causing abnormal CPU usage before taking action.

---

## 4. Memory Bottleneck Investigation

### Commands

`free -h`  
`ps aux --sort=-%mem | head`  
`swapon --show`

### Check

- Available memory
- Swap usage
- Memory-consuming processes

### Key Point

Low available memory combined with increasing swap usage can indicate memory pressure.

---

## 5. Check System Memory Details

### Commands

`cat /proc/meminfo`  
`vmstat 1 5`

### Check

- Memory
- Swap
- System activity
- I/O wait

### Key Point

Use `vmstat` to determine whether the problem is primarily CPU, memory, or I/O related.

---

## 6. Disk Space Problem

### Commands

`df -h`  
`df -i`  
`du -sh /* 2>/dev/null`

### Check

- Filesystem usage
- Inode usage
- Large directories

### Key Point

A filesystem that reaches 100% usage can cause applications and system services to fail.

---

## 7. Disk I/O Bottleneck

### Commands

`iostat -xz 1 5`  
`vmstat 1 5`

### Check

- Device utilisation
- I/O wait
- Read/write activity

### Key Point

High I/O wait or high disk utilisation can indicate a storage bottleneck.

---

## 8. Install Performance Tools

### Commands

`sudo apt update`  
`sudo apt install sysstat`

### Verify

`iostat`

### Key Point

The `sysstat` package provides useful performance-monitoring tools such as `iostat`.

---

## 9. Check Network-Related Performance

### Commands

`ip -s link`  
`ss -s`

### Check

- Packet errors
- Dropped packets
- Socket statistics

### Key Point

Not every "slow server" problem is caused by CPU or memory. Network errors can also affect application performance.

---

## 10. Identify Resource Bottleneck

### Investigation

### CPU

`top`

`ps aux --sort=-%cpu | head`

### Memory

`free -h`

`ps aux --sort=-%mem | head`

### Disk

`iostat -xz 1 5`

### Network

`ip -s link`

### Disk Space

`df -h`

### Key Point

Compare all major resources before deciding the root cause.

---

## 11. High CPU Scenario

### Scenario

Server response becomes slow and CPU usage is unusually high.

### Investigation

`uptime`

`top`

`ps aux --sort=-%cpu | head`

### Identify

- High CPU process
- PID
- Application/service

### Check Service

`systemctl status <service>`

### Check Logs

`journalctl -u <service> -b`

### Action

Investigate why the application is consuming CPU before restarting or terminating it.

### Verification

`top`

`uptime`

Confirm that CPU usage and system load have returned to an acceptable level.

---

## 12. High Memory Scenario

### Scenario

Applications become slow because available memory is very low.

### Investigation

`free -h`

`swapon --show`

`ps aux --sort=-%mem | head`

### Check

- Memory usage
- Swap usage
- Highest memory-consuming processes

### Action

Identify the application causing abnormal memory usage and investigate its logs/configuration.

### Verification

`free -h`

`swapon --show`

---

## 13. High Load Scenario

### Scenario

The load average is unusually high.

### Investigation

`uptime`

`top`

`vmstat 1 5`

### Check

- CPU activity
- I/O wait
- Running processes
- Memory pressure

### Key Point

High load can result from CPU contention, disk I/O, or processes waiting on resources.

---

## 14. Disk Full Scenario

### Scenario

An application stops working because the filesystem is full.

### Investigation

`df -h`

### Find Large Directories

`sudo du -xhd1 / 2>/dev/null | sort -h`

### Check Inodes

`df -i`

### Action

Identify unnecessary or excessive data before deleting anything.

### Verification

`df -h`

Confirm that sufficient free space has been recovered.

---

## 15. Performance Troubleshooting Workflow

### Step 1 — Confirm

`uptime`

### Step 2 — Check CPU

`top`

### Step 3 — Check Memory

`free -h`

### Step 4 — Check Swap

`swapon --show`

### Step 5 — Check Disk Space

`df -h`

### Step 6 — Check Disk I/O

`iostat -xz 1 5`

### Step 7 — Check Network Statistics

`ip -s link`

### Step 8 — Identify Bottleneck

Determine whether the problem is:

- CPU
- Memory
- Disk I/O
- Disk space
- Network
- Application

### Step 9 — Investigate Root Cause

Use logs and service/process information.

### Step 10 — Apply Controlled Fix

Make the smallest appropriate change.

### Step 11 — Verify

Repeat the relevant performance checks.

---

## 16. Performance Troubleshooting Best Practices

- Establish a normal performance baseline.
- Confirm the problem before making changes.
- Check multiple resources.
- Do not assume high load means high CPU.
- Identify the responsible process/application.
- Check logs before restarting services.
- Avoid killing processes without understanding the cause.
- Make one controlled change at a time.
- Verify performance after the change.
- Document the incident and root cause.

---

## 17. Important Commands Summary

### Overall Performance

`uptime`  
`top`  
`vmstat 1 5`

### CPU

`ps aux --sort=-%cpu | head`

### Memory

`free -h`  
`ps aux --sort=-%mem | head`  
`swapon --show`

### Disk Space

`df -h`  
`df -i`  
`du -sh`

### Disk I/O

`iostat -xz 1 5`

### Network

`ip -s link`  
`ss -s`

### Logs

`journalctl -u <service> -b`

---

## Real-Work Performance Troubleshooting Flow

Performance Problem  
↓  
`uptime`  
↓  
`top`  
↓  
`free -h`  
↓  
`df -h`  
↓  
`iostat -xz 1 5`  
↓  
`ip -s link`  
↓  
Identify Bottleneck  
↓  
Investigate Process / Service  
↓  
Check Logs  
↓  
Identify Root Cause  
↓  
Apply Fix  
↓  
Monitor Again  
↓  
Verify Recovery  
↓  
Document Incident

---

## Result

Successfully studied the main Linux system-performance troubleshooting techniques required for server operations.

The module covered:

- CPU bottlenecks
- Memory pressure
- Swap usage
- High load average
- Disk-space problems
- Disk I/O bottlenecks
- Network-related performance issues
- Resource bottleneck identification
- Performance troubleshooting scenarios
- Root-cause investigation
- Recovery verification

## Module Status

**M17 — COMPLETED** ✅
