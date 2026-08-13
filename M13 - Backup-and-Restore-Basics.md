# M13 — Backup & Restore Basics

## Objective

Learn the basic Linux backup and restore techniques commonly used in server operations.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Backup Principles

### Important Concepts

**Backup** — Copy important data to another location so it can be recovered after deletion, corruption, hardware failure, or configuration mistakes.

### Main Backup Types

- Full Backup
- Incremental Backup
- Differential Backup

### Important Rule

A backup is useful only if it can be successfully restored.

---

## 2. Check Files Before Backup

### Commands

`ls -lah`  
`du -sh <directory>`  
`df -h`

### Purpose

Before creating a backup, check:

- What data needs to be backed up.
- How much space it requires.
- Whether the destination has enough free space.

---

## 3. Create Backup Directory

### Commands

`sudo mkdir -p /backup`  
`sudo ls -ld /backup`

### Key Point

Use a dedicated backup location instead of mixing backup files with normal application data.

---

## 4. Copy Files with cp

### Commands

`cp file.txt /backup/`  
`cp -r /source/ /backup/`  
`sudo cp -a /etc/ /backup/etc/`

### Key Point

`cp -a` preserves important file attributes such as permissions and timestamps.

---

## 5. Copy Data with rsync

### Commands

`rsync -av /source/ /backup/`  
`sudo rsync -av /etc/ /backup/etc/`

### Useful Options

`-a` — Archive mode  
`-v` — Verbose output  
`-h` — Human-readable output  
`--delete` — Delete destination files that no longer exist in the source

### Important Warning

Be very careful with `--delete`.

Always verify the source and destination before running it.

---

## 6. Check rsync Backup

### Commands

`rsync -av --dry-run /source/ /backup/`  
`du -sh /source/`  
`du -sh /backup/`

### Key Point

`--dry-run` allows you to see what rsync would change without actually modifying the destination.

---

## 7. Create Compressed Backup with tar

### Commands

`tar -cvf backup.tar /source/`  
`tar -czvf backup.tar.gz /source/`

### Options

`-c` — Create archive  
`-v` — Verbose  
`-f` — Specify archive file  
`-z` — gzip compression

### Common Format

`tar -czvf backup.tar.gz /path/to/data/`

---

## 8. List tar Backup Contents

### Command

`tar -tzvf backup.tar.gz`

### Key Point

Always inspect an archive before relying on it as a backup.

---

## 9. Extract tar Backup

### Commands

`tar -xzvf backup.tar.gz`  
`tar -xzvf backup.tar.gz -C /restore/`

### Key Point

`-C` specifies the destination directory.

---

## 10. Restore Files from tar

### Commands

`mkdir -p /restore`  
`tar -xzvf backup.tar.gz -C /restore/`

### Verify

`ls -lah /restore/`

### Important Rule

Restore into a test directory first when possible.

---

## 11. Backup Important Configuration

### Important Directories

`/etc/`  
`/home/`  
`/var/log/`

### Example

`sudo tar -czvf etc-backup.tar.gz /etc/`

### Key Point

Configuration backups are especially useful before major server changes.

---

## 12. Backup User Data

### Commands

`sudo rsync -av /home/ /backup/home/`  
`sudo tar -czvf home-backup.tar.gz /home/`

### Verify

`ls -lah /backup/`  
`du -sh /backup/home/`

---

## 13. Backup File Permissions

### Commands

`ls -l`  
`stat <file>`

### Key Point

When restoring server data, permissions and ownership can be just as important as the file contents.

---

## 14. Preserve Ownership and Permissions

### Recommended

`sudo rsync -a /source/ /backup/`

### Verify

`ls -ln <file>`  
`stat <file>`

### Key Point

Archive mode helps preserve:

- Permissions
- Ownership
- Timestamps
- Symbolic links

---

## 15. Backup Verification

### Commands

`ls -lh /backup/`  
`du -sh /backup/`  
`tar -tzf backup.tar.gz`

### Optional Integrity Check

`sha256sum backup.tar.gz`

### Verify Later

`sha256sum backup.tar.gz`

The checksum should match the original value if the backup file has not changed.

---

## 16. Restore Verification

After restoring:

### Commands

`ls -lah /restore/`  
`du -sh /restore/`  
`stat /restore/<file>`

### Verify

- Files exist.
- Permissions are correct.
- Ownership is correct.
- File sizes are correct.
- Application data is usable.

---

## 17. Backup Before Configuration Changes

Before changing important server configuration:

### Backup

`sudo cp /etc/fstab /etc/fstab.bak`

### Or

`sudo tar -czvf etc-before-change.tar.gz /etc/`

### Key Point

Always create a recovery point before making risky changes.

---

## 18. Restore Configuration File

### Example

`sudo cp /backup/fstab.bak /etc/fstab`

### Test

`sudo mount -a`

### Verify

`findmnt`

### Important Rule

Always validate the configuration after restoring it.

---

## 19. Simple Backup Script Concept

### Example Command

`rsync -av /home/ /backup/home/`

### Automation

Backup commands can later be automated with:

`cron`

or:

`systemd timers`

These will be covered in scheduled-task/automation practice.

---

## 20. Backup Troubleshooting

### Backup Fails

Check:

`df -h`

Check permissions:

`ls -ld /backup`

Check source:

`ls -lah /source`

Check rsync output:

`rsync -av /source/ /backup/`

### Common Causes

- Insufficient disk space
- Permission denied
- Incorrect source path
- Incorrect destination path
- File changed during backup
- Storage unavailable

---

## 21. Restore Troubleshooting

### Check Backup

`ls -lh backup.tar.gz`

### Test Archive

`tar -tzf backup.tar.gz`

### Restore to Test Directory

`mkdir -p /restore-test`  
`tar -xzvf backup.tar.gz -C /restore-test`

### Verify

`ls -lah /restore-test`

### Key Point

If possible, test the restore before overwriting production data.

---

## 22. Real-Work Backup Workflow

1. Identify critical data.
2. Check available storage.
3. Select backup method.
4. Create backup.
5. Verify backup contents.
6. Store backup separately from the source.
7. Document the backup.
8. Test restoration periodically.

### Core Commands

`df -h`  
`du -sh`  
`rsync -av`  
`tar -czvf`  
`tar -tzf`  
`tar -xzvf`  
`sha256sum`

---

## 23. Backup Safety Rules

- Never treat the original server as the only copy.
- Keep backups on separate storage when possible.
- Verify backups after creation.
- Test restoration regularly.
- Be careful with `rsync --delete`.
- Do not overwrite production data during testing.
- Protect backups containing sensitive information.
- Keep a record of backup dates and locations.
- Before major changes, create a fresh backup.

---

## Result

Successfully studied the main Linux backup and restore techniques required for basic server operations.

The module covered:

- Backup principles
- `cp`
- `rsync`
- `tar`
- Compression
- Backup verification
- File restoration
- Configuration backup
- Permission preservation
- Checksum verification
- Restore testing
- Backup troubleshooting
- Restore troubleshooting
- Real-work backup workflow

## Module Status

**M13 — COMPLETED** ✅
