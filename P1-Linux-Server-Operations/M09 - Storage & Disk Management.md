# M09 — Storage & Disk Management

## Objective

Understand, inspect, configure, mount, manage, and troubleshoot Linux storage, disks, partitions, filesystems, swap, and LVM on Ubuntu Server.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Disk / Block Device Identification

### Commands

`lsblk`  
`lsblk -f`  
`sudo blkid`  
`sudo fdisk -l`

### Key Points

- `lsblk` — Display disks, partitions, and block devices.
- `lsblk -f` — Display filesystem type, UUID, and mount point.
- `blkid` — Display UUID and filesystem type.
- `fdisk -l` — Display disk and partition information.

### Important Device Names

`/dev/sda`  
`/dev/sda1`  
`/dev/sda2`  
`/dev/nvme0n1`  
`/dev/nvme0n1p1`

---

## 2. Disk Capacity & Filesystem Usage

### Commands

`df -h`  
`df -H`  
`df -Th`

### Key Points

- `df -h` — Display filesystem disk usage in human-readable format.
- `df -H` — Display disk usage using decimal units.
- `df -Th` — Display disk usage and filesystem type.

### Lab Evidence

`/dev/sda2`  
`Filesystem: ext4`  
`Size: 30G`  
`Used: 6.6G`  
`Available: 22G`

---

## 3. Directory / File Disk Usage

### Commands

`du -sh <directory>`  
`du -sh *`  
`du -ah <directory>`  
`du -xh --max-depth=1 <directory>`

### Key Points

- `du -sh` — Display total size of a directory.
- `du -sh *` — Display sizes of items in the current directory.
- `du -ah` — Display file and directory sizes.
- `du -xh --max-depth=1` — Identify directories consuming disk space.

---

## 4. Find Large Files

### Commands

`sudo find / -type f -size +500M -ls`  
`sudo find /var -type f -size +100M -ls`  
`sudo du -ah /var | sort -h`

### Key Points

Useful for finding:

- Large log files
- Backup files
- Application data
- Temporary files
- Core dumps

---

## 5. Inode Management

A filesystem can run out of inodes even when free disk space remains.

### Commands

`df -i`  
`df -ih`

### Important Concept

**Disk Space Full ≠ Inode Full**

Both conditions can prevent applications from creating new files.

---

## 6. Partition Information

### Commands

`sudo fdisk -l`  
`sudo parted -l`

### Check

- Disk size
- Partition table
- Partition size
- Partition type
- Partition number

---

## 7. Partition Management — fdisk

### Command

`sudo fdisk /dev/sdb`

### Interactive Commands

`m` — Help  
`p` — Print partition table  
`n` — Create partition  
`d` — Delete partition  
`t` — Change partition type  
`w` — Write changes  
`q` — Quit without saving

### Safety Check

`lsblk`  
`sudo fdisk -l`

Always verify the correct device before modifying a disk.

---

## 8. Partition Management — parted

### Commands

`sudo parted -l`  
`sudo parted /dev/sdb`

### Interactive Commands

`print`  
`mklabel`  
`mkpart`  
`rm`  
`quit`

### Key Point

`parted` can manage partition tables and partitions, including GPT disks.

---

## 9. Filesystem Identification

### Commands

`lsblk -f`  
`sudo blkid`  
`df -Th`

### Common Filesystems

`ext4`  
`xfs`  
`vfat`

### Check

- Filesystem type
- UUID
- Device
- Mount point

---

## 10. Create a Filesystem

### Commands

`sudo mkfs.ext4 /dev/sdb1`  
`sudo mkfs.xfs /dev/sdb1`

### Safety Check

`lsblk`  
`sudo blkid`

**Warning:** `mkfs` is destructive and can erase existing data. Always verify the target device before formatting.

---

## 11. Filesystem Checking — fsck

### Commands

`sudo fsck /dev/sdb1`  
`sudo fsck.ext4 /dev/sdb1`

### Check Mount Status

`findmnt /dev/sdb1`

### Key Points

- `fsck` — Check filesystem consistency.
- It can repair filesystem errors.
- Do not normally repair a mounted filesystem.

---

## 12. ext4 Filesystem Information — tune2fs

### Command

`sudo tune2fs -l /dev/sda2`

### Useful Information

- Filesystem UUID
- Filesystem features
- Mount count
- Reserved blocks
- Filesystem state

---

## 13. Mount a Filesystem

### Commands

`sudo mkdir -p /mnt/data`  
`sudo mount /dev/sdb1 /mnt/data`

### Verify

`findmnt /mnt/data`  
`df -h /mnt/data`

### Key Point

`mount` attaches a filesystem to the Linux directory tree.

---

## 14. Unmount a Filesystem

### Command

`sudo umount /mnt/data`

### Verify

`findmnt /mnt/data`

### If the Filesystem Is Busy

`sudo lsof +f -- /mnt/data`  
`sudo fuser -vm /mnt/data`

These commands identify processes using the filesystem.

---

## 15. Persistent Mount — /etc/fstab

Persistent filesystem mounts are configured in `/etc/fstab`.

### View

`cat /etc/fstab`

### Backup

`sudo cp /etc/fstab /etc/fstab.bak`

### Edit

`sudo nano /etc/fstab`

### Test

`sudo mount -a`

### Verify

`findmnt`  
`df -h`

### Important Rule

Always test `/etc/fstab` with `sudo mount -a` before rebooting.

---

## 16. UUID-Based Mounting

### Commands

`sudo blkid`  
`lsblk -f`

### Example `/etc/fstab` Entry

`UUID=<filesystem-uuid> /mnt/data ext4 defaults 0 2`

### Test

`sudo mount -a`

### Key Point

UUID provides a stable way to identify a filesystem.

---

## 17. Mount Options

### Commands

`mount`  
`findmnt`

### Common Options

`defaults`  
`ro`  
`rw`  
`noexec`  
`nosuid`  
`nodev`

### Key Point

Mount options control how a filesystem is mounted and what operations are permitted.

---

## 18. Disk Health / SMART

### Install

`sudo apt update`  
`sudo apt install smartmontools`

### Check

`sudo smartctl -i /dev/sda`  
`sudo smartctl -a /dev/sda`

### Key Point

SMART information can help identify potential physical disk problems.

---

## 19. Kernel Storage Messages

### Commands

`dmesg | tail -50`  
`dmesg | grep -i error`  
`dmesg | grep -i disk`  
`sudo journalctl -k`

### Useful For

- Disk errors
- I/O errors
- Storage controller problems
- Filesystem errors
- Kernel-level storage events

---

## 20. Swap — Check Status

### Commands

`free -h`  
`swapon --show`  
`cat /proc/swaps`

### Check

- RAM usage
- Swap usage
- Active swap devices or files

---

## 21. Swap — Create Swap File

### Commands

`sudo fallocate -l 2G /swapfile`  
`sudo chmod 600 /swapfile`  
`sudo mkswap /swapfile`  
`sudo swapon /swapfile`

### Verify

`swapon --show`  
`free -h`

---

## 22. Swap — Disable / Remove

### Commands

`sudo swapoff /swapfile`  
`sudo rm /swapfile`

If the swap file is configured in `/etc/fstab`, remove or comment out its entry before deleting it.

---

## 23. LVM Fundamentals

### LVM Structure

Physical Disk  
↓  
Physical Volume (PV)  
↓  
Volume Group (VG)  
↓  
Logical Volume (LV)  
↓  
Filesystem  
↓  
Mount Point

### Key Point

LVM provides flexible storage management and makes it easier to extend storage in many common server scenarios.

---

## 24. LVM Information

### Physical Volumes

`sudo pvs`  
`sudo pvdisplay`

### Volume Groups

`sudo vgs`  
`sudo vgdisplay`

### Logical Volumes

`sudo lvs`  
`sudo lvdisplay`

---

## 25. LVM — Create Physical Volume

### Command

`sudo pvcreate /dev/sdb`

### Verify

`sudo pvs`

### Key Point

`pvcreate` initializes a disk or partition as an LVM Physical Volume.

---

## 26. LVM — Create Volume Group

### Command

`sudo vgcreate vg_data /dev/sdb`

### Verify

`sudo vgs`

### Key Point

`vgcreate` creates a Volume Group from one or more Physical Volumes.

---

## 27. LVM — Create Logical Volume

### Command

`sudo lvcreate -L 10G -n lv_data vg_data`

### Verify

`sudo lvs`

### Key Point

`lvcreate` creates a Logical Volume from available Volume Group space.

---

## 28. LVM — Create Filesystem and Mount

### Create Filesystem

`sudo mkfs.ext4 /dev/vg_data/lv_data`

### Create Mount Point

`sudo mkdir -p /mnt/data`

### Mount

`sudo mount /dev/vg_data/lv_data /mnt/data`

### Verify

`df -h`  
`findmnt /mnt/data`

---

## 29. LVM — Extend Logical Volume

### Extend Logical Volume

`sudo lvextend -L +5G /dev/vg_data/lv_data`

### Extend ext4 Filesystem

`sudo resize2fs /dev/vg_data/lv_data`

### Extend LV and Filesystem Together

`sudo lvextend -r -L +5G /dev/vg_data/lv_data`

### Verify

`sudo lvs`  
`df -h`

---

## 30. LVM — Reduce Logical Volume

For ext4, the filesystem must be reduced before reducing the logical volume.

### Unmount

`sudo umount /mnt/data`

### Check Filesystem

`sudo e2fsck -f /dev/vg_data/lv_data`

### Reduce Filesystem

`sudo resize2fs /dev/vg_data/lv_data <new-size>`

### Reduce Logical Volume

`sudo lvreduce -L <new-size> /dev/vg_data/lv_data`

### Critical Warning

Reducing storage is dangerous.

Always ensure:

- Backup exists
- Filesystem is unmounted
- Filesystem is checked
- New size is large enough for existing data

---

## 31. Disk-Full Troubleshooting

### Step 1 — Check Disk Space

`df -h`

### Step 2 — Check Inodes

`df -i`

### Step 3 — Find Large Directories

`sudo du -xh --max-depth=1 /var`

### Step 4 — Find Large Files

`sudo find /var -type f -size +500M -ls`

### Troubleshooting Flow

`df -h` → Check disk space  
`df -i` → Check inode usage  
`du` → Find large directories  
`find` → Find large files

---

## 32. Deleted File Still Consuming Disk Space

A deleted file can continue consuming disk space if a process still has the file open.

### Command

`sudo lsof +L1`

### Key Concept

File deleted → Process still has it open → Disk space remains used

This is an important real-world Linux troubleshooting case.

---

## 33. Mount Failure Troubleshooting

### Check Devices and Filesystems

`lsblk -f`  
`sudo blkid`

### Check fstab

`cat /etc/fstab`

### Test fstab

`sudo mount -a`

### Check Mounts

`findmnt`

### Check Kernel Messages

`dmesg | tail -50`

### Check Boot Logs

`sudo journalctl -b`

### Common Causes

- Incorrect UUID
- Wrong device
- Wrong filesystem type
- Incorrect mount point
- Incorrect `/etc/fstab`
- Filesystem corruption
- Missing device
- Permission problems

---

## 34. Device-Busy Troubleshooting

### Commands

`sudo lsof /mnt/data`  
`sudo fuser -vm /mnt/data`

### Key Point

These commands identify processes currently using a filesystem.

---

## 35. Storage I/O Error Troubleshooting

### Commands

`dmesg | grep -i error`  
`dmesg | grep -i "I/O"`  
`sudo journalctl -k`

### Possible Causes

- Disk failure
- Storage controller problems
- Virtual disk problems
- Filesystem errors
- Hardware connectivity problems

---

## 36. Storage Safety Rules

### Before Partitioning

`lsblk`  
`sudo fdisk -l`

### Before Formatting

`lsblk -f`  
`sudo blkid`

### Before Editing `/etc/fstab`

`sudo cp /etc/fstab /etc/fstab.bak`

### After Editing `/etc/fstab`

`sudo mount -a`

### Important Rules

- Always verify the correct device.
- Never blindly run `fdisk`.
- Never blindly run `mkfs`.
- Never blindly run `pvcreate`.
- Never blindly run `lvreduce`.
- Backup important data before storage changes.
- Test `/etc/fstab` before rebooting.
- Do not repair a mounted filesystem blindly.
- Document storage changes in production environments.

---

## 37. Lab Storage Evidence

The Ubuntu Server VM storage was inspected using `df -h`.

### Observed Filesystem

`/dev/sda2`  
`Filesystem: ext4`  
`Size: 30G`  
`Used: 6.6G`  
`Available: 22G`

This provided practical experience reading filesystem capacity and disk usage on Ubuntu Server.

---

## 38. Real-Work Storage Troubleshooting Workflow

1. Identify the device
2. Check filesystem
3. Check capacity
4. Check inode usage
5. Check mount status
6. Check `/etc/fstab`
7. Check logs
8. Check disk health
9. Apply fix
10. Verify

### Core Commands

`lsblk`  
`lsblk -f`  
`blkid`  
`df -h`  
`df -i`  
`findmnt`  
`mount`  
`du`  
`find`  
`lsof`  
`dmesg`  
`journalctl`

---

## Result

Successfully studied Linux storage administration and troubleshooting.

The module covered:

- Disk and block-device identification
- Disk capacity and filesystem usage
- Directory and file disk usage
- Large-file investigation
- Inode management
- Partition management
- Filesystem management
- Filesystem checking
- Mount and unmount
- Persistent mounts
- UUID-based configuration
- Mount options
- Disk health / SMART
- Swap
- LVM
- Disk-full troubleshooting
- Deleted-but-open files
- Mount troubleshooting
- I/O troubleshooting
- Storage safety procedures

## Module Status

**M09 — COMPLETED** ✅
