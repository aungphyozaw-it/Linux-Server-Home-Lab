 # M11 — System Monitoring & Performance

## Objective

Learn how to monitor Linux server resources and identify basic performance issues involving CPU, memory, disk I/O, system load, and network interfaces.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. System Uptime & Load

### Commands

`uptime`  
`w`  
`who -b`

### Key Points

- System uptime
- Number of logged-in users
- Load average
- Last boot time

### Load Average

Linux shows:

- 1-minute load
- 5-minute load
- 15-minute load

Check CPU count with:

`nproc`

---

## 2. CPU Monitoring

### Commands

`lscpu`  
`nproc`  
`top`

### Check

- CPU usage
- CPU cores
- Load average
- CPU utilisation

### Key Point

Use CPU information together with load average when investigating performance.

---

## 3. Memory Monitoring

### Commands

`free -h`  
`free -m`  
`cat /proc/meminfo`

### Check

- Total RAM
- Used RAM
- Available RAM
- Cache
- Swap

### Key Point

`available` memory is more useful than looking only at `free` memory when assessing current memory pressure.

---

## 4. Swap Monitoring

### Commands

`free -h`  
`swapon --show`  
`cat /proc/swaps`

### Check

- Total swap
- Used swap
- Available swap

### Key Point

High swap usage can indicate memory pressure.

---

## 5. Virtual Memory & System Activity

### Commands

`vmstat`  
`vmstat 1`  
`vmstat 1 5`

### Monitor

- Memory
- Swap
- Processes
- I/O
- CPU
- System activity

### Key Point

`vmstat` provides a quick overall view of system performance.

---

## 6. Disk Space Monitoring

### Commands

`df -h`  
`df -Th`  
`du -sh <directory>`

### Check

- Filesystem usage
- Available space
- Filesystem type
- Directory size

### Key Point

`df` shows filesystem capacity while `du` helps identify where disk space is being consumed.

---

## 7. Disk I/O Performance

### Install

`sudo apt update`  
`sudo apt install sysstat`

### Commands

`iostat`  
`iostat -xz 1`  
`iostat -xz 1 5`

### Monitor

- Read/write activity
- I/O wait
- Device utilisation
- Disk throughput

### Key Point

High disk utilisation or I/O wait can indicate a storage performance bottleneck.

---

## 8. Network Interface Monitoring

### Commands

`ip -s link`  
`ip -br link`

### Check

- RX packets
- TX packets
- RX errors
- TX errors
- Dropped packets
- Interface state

### Key Point

Network errors and dropped packets can indicate interface, cable, driver, or network problems.

---

## 9. System Resource Overview

### Commands

`uptime`  
`free -h`  
`df -h`  
`vmstat 1 5`  
`iostat -xz 1 5`  
`ip -s link`

### Purpose

Use these commands together for a quick Linux server health check.

---

## 10. Performance Baseline

### Record

`uptime`  
`free -h`  
`df -h`  
`vmstat 1 5`  
`iostat -xz 1 5`  
`ip -s link`

### Key Point

A baseline gives you a normal performance reference.

Later, abnormal results can be compared against the baseline.

---

## 11. Basic Performance Investigation

### Scenario

Users report that the server is slow.

### Step 1 — Check Load

`uptime`

### Step 2 — Check CPU

`top`  
`lscpu`

### Step 3 — Check Memory

`free -h`

### Step 4 — Check Swap

`swapon --show`

### Step 5 — Check Disk Space

`df -h`

### Step 6 — Check Disk I/O

`iostat -xz 1 5`

### Step 7 — Check Network Interface

`ip -s link`

### Step 8 — Compare Results

Compare the current results against the normal system baseline.

---

## 12. Common Performance Indicators

### CPU

High CPU utilisation or consistently high load may indicate CPU pressure.

### Memory

Low available memory and increasing swap usage may indicate memory pressure.

### Disk

High device utilisation or I/O wait may indicate storage bottlenecks.

### Network

Increasing errors or dropped packets may indicate network-interface problems.

### Disk Space

A filesystem approaching 100% usage can cause application and system problems.

---

## 13. Important Monitoring Commands Summary

### System

`uptime`  
`w`  
`who -b`  
`nproc`

### CPU

`lscpu`  
`top`

### Memory

`free -h`  
`cat /proc/meminfo`

### Swap

`swapon --show`  
`cat /proc/swaps`

### System Performance

`vmstat 1 5`

### Disk Space

`df -h`  
`df -Th`  
`du -sh <directory>`

### Disk I/O

`iostat -xz 1 5`

### Network Interface

`ip -s link`  
`ip -br link`

---

## Real-Work Performance Monitoring Flow

Server Performance Problem  
↓  
`uptime`  
↓  
Check CPU  
↓  
`top`  
↓  
Check Memory / Swap  
↓  
`free -h`  
↓  
`swapon --show`  
↓  
Check Disk Space  
↓  
`df -h`  
↓  
Check Disk I/O  
↓  
`iostat -xz 1 5`  
↓  
Check Network Interface  
↓  
`ip -s link`  
↓  
Compare With Baseline  
↓  
Identify Performance Area

---

## Result

Successfully studied the main Linux system monitoring and performance concepts required for server operations.

The module covered:

- System uptime
- Load average
- CPU monitoring
- Memory monitoring
- Swap monitoring
- Virtual memory statistics
- Disk space monitoring
- Disk I/O performance
- Network interface statistics
- Performance baseline
- Basic performance investigation

## Module Status

**M11 — COMPLETED** ✅
