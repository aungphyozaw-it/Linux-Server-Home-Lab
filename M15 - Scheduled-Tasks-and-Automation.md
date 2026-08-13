# M15 — Scheduled Tasks & Automation

## Objective

Learn how to schedule recurring and one-time tasks on Ubuntu Server using `cron`, `crontab`, and `systemd timers`.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab

---

## 1. Check Current Cron Jobs

### Commands

`crontab -l`  
`sudo crontab -l`

### Key Points

- `crontab -l` — Shows the current user's scheduled jobs.
- `sudo crontab -l` — Shows root's scheduled jobs.

---

## 2. Edit Cron Jobs

### Command

`crontab -e`

### Root Cron

`sudo crontab -e`

### Key Point

Use the user's crontab for user-level tasks and root's crontab for tasks requiring root privileges.

---

## 3. Cron Format

### Structure

`* * * * * command`

### Fields

1. Minute
2. Hour
3. Day of Month
4. Month
5. Day of Week

### Common Examples

`0 2 * * * command` — Every day at 02:00

`*/5 * * * * command` — Every 5 minutes

`0 0 * * 0 command` — Every Sunday at midnight

---

## 4. Create a Simple Scheduled Task

### Example

`echo "Backup completed" >> /tmp/backup.log`

Schedule it:

`*/5 * * * * echo "Backup completed" >> /tmp/backup.log`

### Verify

`cat /tmp/backup.log`

### Key Point

For real server work, scheduled commands should normally use absolute paths and proper logging.

---

## 5. Remove Cron Jobs

### Command

`crontab -e`

Remove the unwanted entry and save the file.

### Verify

`crontab -l`

---

## 6. Cron Service Status

### Commands

`systemctl status cron`  
`systemctl is-active cron`  
`systemctl is-enabled cron`

### Start / Restart

`sudo systemctl start cron`  
`sudo systemctl restart cron`

---

## 7. Cron Logs

### Commands

`journalctl -u cron`  
`journalctl -u cron -b`  
`grep -i cron /var/log/syslog`

### Key Point

Use logs when a scheduled task does not run as expected.

---

## 8. One-Time Tasks — at

### Check

`atq`

### Create Task

`at 23:00`

Then enter the command.

### Example

`echo "Test task" >> /tmp/at-test.log`

Finish with:

`Ctrl+D`

### View Scheduled Tasks

`atq`

### Remove Task

`atrm <job-id>`

### Key Point

`at` is useful for one-time tasks rather than recurring schedules.

---

## 9. systemd Timer Basics

### Commands

`systemctl list-timers`  
`systemctl list-timers --all`

### Key Point

`systemd timers` can schedule services and are commonly used on modern Linux systems.

---

## 10. Check Scheduled Timers

### Command

`systemctl list-timers --all`

### Useful Information

- Next execution
- Last execution
- Time remaining
- Timer unit
- Service unit

---

## 11. Scheduled Task Troubleshooting

### Step 1 — Check Cron

`systemctl status cron`

### Step 2 — Check User Crontab

`crontab -l`

### Step 3 — Check Root Crontab

`sudo crontab -l`

### Step 4 — Check Logs

`journalctl -u cron -b`

### Step 5 — Check Script Permissions

`ls -l /path/to/script`

### Step 6 — Test Manually

`/path/to/script`

### Step 7 — Verify Output

`cat /path/to/logfile`

---

## 12. Script-Based Automation

### Check Script

`ls -l /path/to/script`

### Make Executable

`chmod +x /path/to/script`

### Run Manually

`/path/to/script`

### Run With Shell

`bash /path/to/script.sh`

### Key Point

Always test an automation script manually before scheduling it.

---

## 13. Logging Automated Tasks

### Example

`/path/to/script.sh >> /var/log/my-script.log 2>&1`

### Meaning

`>>` — Append normal output

`2>&1` — Send error output to the same log

### Verify

`sudo tail -f /var/log/my-script.log`

---

## 14. Important Automation Safety Rules

- Test commands manually before scheduling.
- Use absolute paths in cron jobs.
- Redirect output to logs.
- Avoid destructive commands without testing.
- Check file permissions.
- Use least privilege.
- Be careful with `sudo`.
- Monitor scheduled jobs.
- Keep scripts documented.
- Verify the result after execution.

---

## 15. Real-Work Example — Scheduled Backup

### Manual Test

`sudo rsync -av /home/ /backup/home/`

### Example Cron

`0 2 * * * /usr/bin/rsync -av /home/ /backup/home/ >> /var/log/backup.log 2>&1`

### Check

`crontab -l`

### Monitor

`sudo tail -f /var/log/backup.log`

### Verify

`du -sh /backup/home/`

---

## 16. Real-Work Automation Troubleshooting Flow

Scheduled Task  
↓  
Check schedule  
↓  
`crontab -l`  
↓  
Check service  
↓  
`systemctl status cron`  
↓  
Check logs  
↓  
`journalctl -u cron -b`  
↓  
Run command manually  
↓  
Check permissions  
↓  
Check output/log  
↓  
Fix  
↓  
Verify next execution

---

## 17. Important Commands Summary

### Cron

`crontab -l`  
`crontab -e`  
`sudo crontab -l`  
`sudo crontab -e`

### Cron Service

`systemctl status cron`  
`systemctl is-active cron`  
`systemctl is-enabled cron`

### Cron Logs

`journalctl -u cron -b`  
`grep -i cron /var/log/syslog`

### One-Time Tasks

`atq`  
`atrm <job-id>`

### Systemd Timers

`systemctl list-timers`  
`systemctl list-timers --all`

### Script Testing

`ls -l /path/to/script`  
`chmod +x /path/to/script`  
`bash /path/to/script.sh`

---

## Result

Successfully studied the main Linux scheduled-task and automation methods used in server operations.

The module covered:

- Cron
- Crontab
- Cron scheduling syntax
- Cron service management
- Cron logs
- One-time tasks with `at`
- Systemd timers
- Script automation
- Automated-task logging
- Scheduled backup example
- Automation troubleshooting

## Module Status

**M14 — COMPLETED** ✅
