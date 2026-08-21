# M18 — Disk & Storage Troubleshooting

## Objective

Learn how to diagnose and troubleshoot Linux disk, filesystem, partition, mount, and storage-related problems.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Check Disk Devices

### Commands

`lsblk`  
`sudo fdisk -l`  
`sudo blkid`

### Check

- Disk devices
- Partitions
- Filesystem type
- UUID
- Mount points

### Key Point

Always identify the correct disk and partition before performing storage operations.

---

## 2. Check Filesystem Usage

### Commands

`df -h`  
`df -Th`

### Check

- Used space
- Available space
- Filesystem type
- Mount point

### Key Point

A filesystem reaching 100% usage can cause applications and services to fail.

---

## 3. Find Large Files and Directories

### Commands

`sudo du -xhd1 / 2>/dev/null | sort -h`

`sudo du -sh /var/* 2>/dev/null | sort -h`

### Find Large Files

`sudo find / -type f -size +500M -exec ls -lh {} \; 2>/dev/null`

### Key Point

Identify what is consuming storage before deleting anything.

---

## 4. Check Inode Usage

### Commands

`df -i`  
`df -ih`

### Key Point

A filesystem can have free disk space but still fail if all inodes are exhausted.

---

## 5. Check Mount Points

### Commands

`findmnt`  
`mount`  
`cat /etc/fstab`

### Check

- Mounted filesystems
- Mount points
- Filesystem types
- Persistent mount configuration

---

## 6. Check a Specific Mount

### Command

`findmnt <mount-point>`

### Example

`findmnt /`

### Key Point

Useful for confirming which device and filesystem are associated with a mount point.

---

## 7. Troubleshoot Missing Mount

### Scenario

A filesystem is expected to be mounted but is not available.

### Check

`lsblk`

`findmnt`

`cat /etc/fstab`

### Check Errors

`journalctl -b | grep -i mount`

### Investigation

Determine whether the problem is:

- Incorrect `/etc/fstab`
- Missing device
- Wrong UUID
- Filesystem problem
- Mount-point problem

---

## 8. Test `/etc/fstab`

### Command

`sudo mount -a`

### Key Point

`mount -a` attempts to mount filesystems configured in `/etc/fstab`.

Always verify the result after changing `/etc/fstab`.

### Verify

`findmnt`

`df -h`

---

## 9. Check Filesystem Errors

### Important Commands

`sudo fsck -N /dev/sdX1`

### Key Point

Do not run filesystem repair commands blindly on a mounted filesystem.

For actual repair, the filesystem generally needs to be unmounted or checked from an appropriate recovery environment.

---

## 10. Check Disk Health — SMART

### Install

`sudo apt update`

`sudo apt install smartmontools`

### Check SMART Support

`sudo smartctl -i /dev/sda`

### SMART Health

`sudo smartctl -H /dev/sda`

### Detailed Information

`sudo smartctl -a /dev/sda`

### Key Point

SMART information can help identify possible physical disk-health problems.

---

## 11. Check Disk I/O Problems

### Commands

`iostat -xz 1 5`

`vmstat 1 5`

### Check

- I/O wait
- Device utilisation
- Read/write activity

### Key Point

High I/O wait may indicate storage performance problems.

---

## 12. Check Kernel Storage Messages

### Commands

`dmesg | grep -i -E "error|fail|ata|disk|I/O"`

`journalctl -k -b`

### Check

- I/O errors
- Disk errors
- Device detection problems
- Filesystem errors

### Key Point

Kernel logs are important evidence when investigating storage failures.

---

## 13. Check Open Files on a Filesystem

### Commands

`sudo lsof +L1`

`sudo lsof /var`

### Key Point

A deleted file can continue consuming disk space while a process still has it open.

---

## 14. Deleted Files Still Using Disk Space

### Scenario

`df -h` shows low free space, but `du` does not show enough data to explain it.

### Check

`sudo lsof +L1`

### Identify

Look for files marked as:

`(deleted)`

### Investigation

Identify the process holding the deleted file open.

### Key Point

Restarting the affected application may release the space, but investigate the reason before taking action.

---

## 15. Read-Only Filesystem

### Scenario

An application reports:

`Read-only file system`

### Check

`mount | grep " ro,"`

`findmnt -o TARGET,SOURCE,FSTYPE,OPTIONS`

### Check Kernel Messages

`dmesg | tail -50`

### Investigation

Look for filesystem or I/O errors.

### Key Point

Do not simply force the filesystem back to read-write without understanding why it became read-only.

---

## 16. Disk Full Troubleshooting

### Scenario

Application fails because storage is full.

### Step 1

`df -h`

### Step 2

`df -i`

### Step 3

Find Large Directories

`sudo du -xhd1 / 2>/dev/null | sort -h`

### Step 4

Find Large Files

`sudo find / -type f -size +500M -exec ls -lh {} \; 2>/dev/null`

### Step 5

Check Deleted Files

`sudo lsof +L1`

### Step 6

Identify Root Cause

Determine whether the problem is:

- Logs
- Application data
- Temporary files
- Deleted-but-open files
- Inode exhaustion

### Step 7

Apply Controlled Fix

Remove or rotate data only after confirming it is safe.

### Step 8

Verify

`df -h`

---

## 17. Mount Failure Troubleshooting

### Scenario

A storage filesystem does not mount after reboot.

### Check

`lsblk -f`

`cat /etc/fstab`

`findmnt`

### Test

`sudo mount -a`

### Check Logs

`journalctl -b | grep -i mount`

### Root Cause Examples

- Incorrect UUID
- Wrong filesystem type
- Incorrect mount point
- Missing disk
- Filesystem error

### Verify

`findmnt`

`df -h`

---

## 18. Storage Troubleshooting Workflow

### Step 1 — Identify Devices

`lsblk`

### Step 2 — Check Filesystems

`lsblk -f`

### Step 3 — Check Usage

`df -h`

### Step 4 — Check Inodes

`df -i`

### Step 5 — Check Mounts

`findmnt`

### Step 6 — Check Configuration

`cat /etc/fstab`

### Step 7 — Check Kernel Logs

`journalctl -k -b`

### Step 8 — Check Disk Health

`sudo smartctl -H /dev/sda`

### Step 9 — Identify Root Cause

Determine whether the issue is:

- Capacity
- Inodes
- Mount
- Filesystem
- Disk health
- I/O
- Configuration

### Step 10 — Apply Controlled Fix

Perform the appropriate recovery action.

### Step 11 — Verify

Confirm:

- Device detected
- Filesystem mounted
- Correct capacity
- Application access restored
- No new storage errors

---

## 19. Important Commands Summary

### Disk / Partition

`lsblk`  
`sudo fdisk -l`  
`sudo blkid`

### Filesystem

`df -h`  
`df -Th`  
`df -i`

### Mount

`findmnt`  
`mount`  
`cat /etc/fstab`  
`sudo mount -a`

### Disk Usage

`du -sh`  
`find`

### Filesystem Investigation

`sudo fsck -N /dev/sdX1`

### Disk Health

`sudo smartctl -H /dev/sda`  
`sudo smartctl -a /dev/sda`

### I/O

`iostat -xz 1 5`  
`vmstat 1 5`

### Logs

`journalctl -k -b`  
`dmesg`

### Open Files

`sudo lsof +L1`  
`sudo lsof /var`

---

## Real-Work Storage Troubleshooting Flow

Storage Problem  
↓  
Identify Disk  
↓  
`lsblk`  
↓  
Check Filesystem  
↓  
`lsblk -f`  
↓  
Check Capacity  
↓  
`df -h`  
↓  
Check Inodes  
↓  
`df -i`  
↓  
Check Mount  
↓  
`findmnt`  
↓  
Check `/etc/fstab`  
↓  
Check Kernel Logs  
↓  
`journalctl -k -b`  
↓  
Check Disk Health  
↓  
`smartctl`  
↓  
Identify Root Cause  
↓  
Apply Controlled Recovery  
↓  
Verify Filesystem / Mount / Application  
↓  
Document Result

---

## Result

Successfully studied the main Linux disk and storage troubleshooting techniques required for server operations.

The module covered:

- Disk and partition identification
- Filesystem usage
- Inode exhaustion
- Large-file investigation
- Mount troubleshooting
- `/etc/fstab` troubleshooting
- Filesystem error investigation
- SMART disk-health checks
- Disk I/O problems
- Kernel storage errors
- Deleted-but-open files
- Read-only filesystem troubleshooting
- Disk-full recovery
- Storage mount failure recovery
- Structured storage troubleshooting

## Module Status

**M18 — COMPLETED** ✅
