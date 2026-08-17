# M08 — Scheduled Tasks / Cron
 
## Objective
 
Understand and manage scheduled tasks on Ubuntu Server using cron, crontab, system cron directories, and one-time scheduling with `at`.
 
## Lab Environment
 
 
- Ubuntu Server
 
- Oracle VirtualBox
 
- Linux Server Home Lab
 

  
# 1. Cron Fundamentals
 
`cron` is a Linux service used to automatically execute commands or scripts at scheduled times.
 
### Check Cron Service
 `systemctl status cron ` 
### Check Whether Cron Is Running
 `systemctl is-active cron ` 
### Check Whether Cron Starts at Boot
 `systemctl is-enabled cron ` 
### Purpose
 
Use these commands to verify that the cron scheduler is running correctly.
  
# 2. Crontab
 
`crontab` is used to create and manage scheduled tasks for a user.
 
### Edit User Crontab
 `crontab -e ` 
**Purpose:** Create or modify scheduled tasks for the current user.
 
### List User Crontab
 `crontab -l ` 
**Purpose:** Display the current user's scheduled tasks.
 
### Remove User Crontab
 `crontab -r ` 
**Purpose:** Remove all scheduled tasks for the current user.
 
 
Use `crontab -r` carefully because it removes the user's entire crontab.
 
  
# 3. Cron Schedule Syntax
 
A standard cron entry contains five time fields:
 `┌──────── minute (0-59) │ ┌────── hour (0-23) │ │ ┌──── day of month (1-31) │ │ │ ┌── month (1-12) │ │ │ │ ┌ day of week (0-7) │ │ │ │ │ * * * * * command ` 
### Example
 `0 2 * * * /home/aung/backup.sh ` 
**Meaning:** Run `backup.sh` every day at 02:00.
  
# 4. Cron Time Fields
 
### Minute
 `0-59 ` 
### Hour
 `0-23 ` 
### Day of Month
 `1-31 ` 
### Month
 `1-12 ` 
### Day of Week
 `0-7 ` 
`0` and `7` generally represent Sunday.
  
# 5. Common Cron Operators
 
Cron supports several useful scheduling operators.
 
### Every Value
 `* ` 
Example:
 `* * * * * command ` 
**Meaning:** Run every minute.
 
### List
 `1,15,30 ` 
Example:
 `1,15,30 * * * * command ` 
**Meaning:** Run at minutes 1, 15, and 30.
 
### Range
 `1-5 ` 
Example:
 `0 9 * * 1-5 command ` 
**Meaning:** Run at 09:00 Monday through Friday.
 
### Step
 `*/10 ` 
Example:
 `*/10 * * * * command ` 
**Meaning:** Run every 10 minutes.
  
# 6. Practical Cron Examples
 
### Every Minute
 `* * * * * /path/to/script.sh ` 
### Every 5 Minutes
 `*/5 * * * * /path/to/script.sh ` 
### Every Hour
 `0 * * * * /path/to/script.sh ` 
### Every Day at 02:00
 `0 2 * * * /path/to/script.sh ` 
### Every Sunday at 03:00
 `0 3 * * 0 /path/to/script.sh ` 
### Every Weekday at 09:00
 `0 9 * * 1-5 /path/to/script.sh `  
# 7. Redirect Cron Output to a Log
 
Scheduled jobs should often write their output to a log file.
 
### Command
 `0 2 * * * /path/to/script.sh >> /var/log/backup.log 2>&1 ` 
### Meaning
 `>>  ` 
Append normal output to the log.
 `2>&1 ` 
Send error output to the same log.
  
# 8. Run a Script from Cron
 
Before scheduling a script, make sure it can execute correctly.
 
### Check File
 `ls -l /path/to/script.sh ` 
### Make Script Executable
 `chmod +x /path/to/script.sh ` 
### Test Script Manually
 `/path/to/script.sh ` 
or:
 `bash /path/to/script.sh ` 
### Purpose
 
Always test the command/script manually before putting it into cron.
  
# 9. System-Wide Cron
 
Linux also provides system-wide scheduled tasks.
 
### View `/etc/crontab`
 `cat /etc/crontab ` 
### Purpose
 
Display the system-wide cron configuration.
 
Unlike a user's crontab, `/etc/crontab` includes an additional field for the user that should execute the command.
 
Example structure:
 `minute hour day month weekday user command `  
# 10. Cron Directories
 
Ubuntu provides predefined directories for periodic jobs.
 
### Hourly
 `ls -la /etc/cron.hourly/ ` 
### Daily
 `ls -la /etc/cron.daily/ ` 
### Weekly
 `ls -la /etc/cron.weekly/ ` 
### Monthly
 `ls -la /etc/cron.monthly/ ` 
### Purpose
 
Scripts placed in these directories are executed according to their respective schedules.
  
# 11. Cron Service Management
 
### Start Cron
 `sudo systemctl start cron ` 
### Stop Cron
 `sudo systemctl stop cron ` 
### Restart Cron
 `sudo systemctl restart cron ` 
### Enable Cron at Boot
 `sudo systemctl enable cron ` 
### Enable and Start Cron
 `sudo systemctl enable --now cron `  
# 12. Cron Logs
 
Logs are important when a scheduled task does not run as expected.
 
### View Cron Logs
 `sudo journalctl -u cron ` 
### View Recent Cron Logs
 `sudo journalctl -u cron -n 50 ` 
### Follow Cron Logs
 `sudo journalctl -u cron -f ` 
### Search System Logs for Cron
 `sudo grep CRON /var/log/syslog ` 
 
Log locations can vary depending on the Ubuntu configuration.
 
  
# 13. One-Time Scheduled Tasks — `at`
 
`at` is used for jobs that should run once at a specified time.
 
### Check `at` Installation
 `dpkg -l | grep at ` 
### Schedule a One-Time Job
 `echo "command" | at 23:00 ` 
### List Scheduled `at` Jobs
 `atq ` 
### Remove an `at` Job
 `atrm <job-id> ` 
### Purpose
 
Use `at` for one-time tasks rather than recurring tasks.
  
# 14. Cron vs `at`
 
  
 
Tool
 
Purpose
 
   
 
`cron`
 
Recurring scheduled tasks
 
 
 
`crontab`
 
Manage recurring user tasks
 
 
 
`/etc/crontab`
 
System-wide scheduled tasks
 
 
 
`/etc/cron.daily/`
 
Daily jobs
 
 
 
`/etc/cron.weekly/`
 
Weekly jobs
 
 
 
`/etc/cron.monthly/`
 
Monthly jobs
 
 
 
`at`
 
One-time scheduled tasks
 
  
  
# 15. Scheduled Backup Example
 
A common real-world use of cron is automated backup.
 
### Example
 `0 2 * * * /usr/local/bin/backup.sh >> /var/log/backup.log 2>&1 ` 
**Meaning:**
 
Run the backup script every day at 02:00 and record its output in:
 `/var/log/backup.log `  
# 16. Scheduled Maintenance Example
 
Cron can also be used for regular maintenance tasks.
 
### Example
 `0 4 * * 0 /usr/local/bin/maintenance.sh >> /var/log/maintenance.log 2>&1 ` 
**Meaning:**
 
Run the maintenance script every Sunday at 04:00.
  
# 17. Cron Troubleshooting
 
When a cron job does not execute, investigate systematically.
 
### Step 1 — Check Cron Service
 `systemctl status cron ` 
### Step 2 — Check Crontab
 `crontab -l ` 
### Step 3 — Check Script Permissions
 `ls -l /path/to/script.sh ` 
### Step 4 — Test Script Manually
 `bash /path/to/script.sh ` 
### Step 5 — Check Cron Logs
 `sudo journalctl -u cron ` 
### Step 6 — Check System Logs
 `sudo grep CRON /var/log/syslog ` 
### Step 7 — Check Script Paths
 
Cron jobs should preferably use absolute paths.
 
Example:
 `0 2 * * * /usr/local/bin/backup.sh ` 
rather than relying on a relative path.
  
# 18. Important Cron Troubleshooting Issues
 
Common causes of failed cron jobs:
 
 
- Cron service is not running.
 
- Incorrect cron syntax.
 
- Script is not executable.
 
- Incorrect file path.
 
- Relative paths are used.
 
- Required environment variables are missing.
 
- Permission problems.
 
- Script works manually but fails under cron.
 
- Output/errors are not being logged.
 
- User does not have permission to execute the command.
 

  
# 19. Lab Practice
 
Scheduled-task administration was studied as part of Linux server operations.
 
Important commands for practice include:
 `crontab -l crontab -e systemctl status cron sudo journalctl -u cron ` 
The module focuses on understanding how Linux automatically executes recurring and one-time administrative tasks.
  
# 20. Real-Work Skills
 
Scheduled tasks are commonly used for:
 
 
- Automated backups
 
- Log cleanup
 
- Monitoring scripts
 
- Database maintenance
 
- Report generation
 
- Temporary maintenance jobs
 
- File synchronization
 
- System housekeeping
 

 
Always ensure scheduled tasks are:
 
 
- Tested manually
 
- Logged
 
- Permission-safe
 
- Using correct absolute paths
 
- Scheduled during appropriate maintenance windows
 

  
# Commands Summary
 
  
 
Title
 
Commands
 
   
 
Cron Service
 
`systemctl status cron`, `systemctl is-active cron`, `systemctl is-enabled cron`
 
 
 
User Crontab
 
`crontab -e`, `crontab -l`, `crontab -r`
 
 
 
System Cron
 
`cat /etc/crontab`
 
 
 
Cron Directories
 
`ls -la /etc/cron.hourly/`, `daily/`, `weekly/`, `monthly/`
 
 
 
Cron Service Management
 
`systemctl start/stop/restart/enable cron`
 
 
 
Cron Logs
 
`journalctl -u cron`, `grep CRON /var/log/syslog`
 
 
 
One-Time Jobs
 
`at`, `atq`, `atrm`
 
 
 
Script Permissions
 
`ls -l`, `chmod +x`
 
 
 
Script Testing
 
`bash /path/to/script.sh`
 
  
  
# Result
 
Successfully studied Linux scheduled-task management using cron, crontab, system cron directories, and one-time scheduling with `at`.
 
The module covered:
 
 
- Cron fundamentals
 
- Crontab management
 
- Cron schedule syntax
 
- System-wide cron
 
- Periodic cron directories
 
- Cron service management
 
- Cron logs
 
- One-time jobs
 
- Automated backup and maintenance examples
 
- Scheduled-task troubleshooting
 

 
# Module Status
 
**M08 — COMPLETED** ✅
