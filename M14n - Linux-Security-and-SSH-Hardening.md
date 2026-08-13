# M14 — Linux Security & SSH Hardening

## Objective

Learn the main Linux security practices required to protect and securely administer an Ubuntu Server.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab
- SSH

---

## 1. Check Current User

### Commands

`whoami`  
`id`  
`groups`

### Key Point

Always know which user you are operating as before performing administrative tasks.

---

## 2. User Privileges

### Commands

`sudo -l`  
`id`  
`groups`

### Check Sudo Group

`getent group sudo`

### Key Point

Use `sudo` only when administrative privileges are required.

---

## 3. File Permissions

### Commands

`ls -l`  
`stat <file>`  
`chmod 640 <file>`  
`chmod 750 <directory>`

### Key Point

Use the minimum permissions required.

Avoid unnecessary `777` permissions.

---

## 4. File Ownership

### Commands

`ls -l`  
`sudo chown <user>:<group> <file>`  
`sudo chown -R <user>:<group> <directory>`

### Verify

`ls -l`

---

## 5. SSH Service Security

### Check SSH

`systemctl status ssh`

### Check SSH Port

`sudo ss -tulpn | grep :22`

### Check Configuration

`sudo sshd -t`

### Key Point

Always test the SSH configuration before restarting SSH.

---

## 6. SSH Configuration

### Configuration File

`/etc/ssh/sshd_config`

### Backup Before Changes

`sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak`

### Edit

`sudo nano /etc/ssh/sshd_config`

### Test Configuration

`sudo sshd -t`

### Apply Changes

`sudo systemctl reload ssh`

### Verify

`systemctl status ssh`

---

## 7. SSH Security Settings

### Important Settings

`PermitRootLogin no`

`PasswordAuthentication yes`

### Key Point

For production environments, SSH key authentication is preferred over password authentication.

Do not disable password authentication until SSH key login has been tested successfully.

---

## 8. SSH Key Authentication

### Generate Key

`ssh-keygen`

### Copy Public Key

`ssh-copy-id <user>@<server-ip>`

### Test Login

`ssh <user>@<server-ip>`

### Key Files

`~/.ssh/id_ed25519` — Private key

`~/.ssh/id_ed25519.pub` — Public key

### Important

Never share the private key.

---

## 9. SSH Key Permissions

### Commands

`chmod 700 ~/.ssh`  
`chmod 600 ~/.ssh/authorized_keys`

### Verify

`ls -ld ~/.ssh`  
`ls -l ~/.ssh/authorized_keys`

---

## 10. Firewall — UFW

### Check Status

`sudo ufw status`

### Detailed Status

`sudo ufw status verbose`

### Enable

`sudo ufw enable`

### Allow SSH

`sudo ufw allow ssh`

### Allow Specific Port

`sudo ufw allow 22/tcp`

### Remove Rule

`sudo ufw delete allow 22/tcp`

### Key Point

Before enabling a firewall on a remote server, make sure the required management access is allowed.

---

## 11. Check Firewall Rules

### Commands

`sudo ufw status numbered`  
`sudo ufw status verbose`

### Key Point

Review firewall rules regularly and remove unnecessary access.

---

## 12. Check Open Ports

### Command

`sudo ss -tulpn`

### Key Point

Only required services should normally be exposed.

---

## 13. Check Listening Services

### Commands

`systemctl --type=service --state=running`  
`sudo ss -tulpn`

### Purpose

Compare running services with listening network ports.

---

## 14. Check Failed Login Attempts

### Commands

`sudo grep -i "failed" /var/log/auth.log`  
`sudo grep -i "invalid user" /var/log/auth.log`

### Key Point

Useful for identifying repeated SSH authentication attempts.

---

## 15. Check Successful SSH Logins

### Commands

`last`  
`lastlog`

### Key Point

Use these commands during security investigations to identify recent user login activity.

---

## 16. Security Updates

### Commands

`sudo apt update`  
`sudo apt upgrade`

### Check Upgradable Packages

`apt list --upgradable`

### Key Point

Keeping the operating system updated is one of the basic server-security practices.

---

## 17. Check OS Information

### Commands

`cat /etc/os-release`  
`uname -r`

### Key Point

Useful when checking the Ubuntu version and running kernel version during troubleshooting and security maintenance.

---

## 18. Security Troubleshooting Workflow

### Scenario

Unexpected SSH access or suspicious login activity is detected.

### Step 1 — Check Current Users

`who`  
`w`

### Step 2 — Check Login History

`last`

### Step 3 — Check Authentication Logs

`sudo grep -i "failed" /var/log/auth.log`

### Step 4 — Check Open Ports

`sudo ss -tulpn`

### Step 5 — Check Firewall

`sudo ufw status verbose`

### Step 6 — Check Running Services

`systemctl --type=service --state=running`

### Step 7 — Investigate Before Making Changes

Identify the affected account, service, port, or process before taking corrective action.

---

## 19. Linux Security Best Practices

- Use individual user accounts.
- Avoid direct root login over SSH.
- Use `sudo` for administrative tasks.
- Use SSH keys where possible.
- Keep private keys protected.
- Apply security updates regularly.
- Use a firewall.
- Expose only required ports.
- Review authentication logs.
- Use strong file permissions.
- Remove unnecessary services.
- Back up configuration before security changes.
- Test SSH configuration before restarting the service.

---

## 20. Important Commands Summary

### User & Privilege

`whoami`  
`id`  
`groups`  
`sudo -l`

### Permissions

`ls -l`  
`chmod`  
`chown`  
`stat`

### SSH

`systemctl status ssh`  
`sudo sshd -t`  
`sudo ss -tulpn | grep :22`  
`ssh-keygen`  
`ssh-copy-id`

### Firewall

`sudo ufw status`  
`sudo ufw status verbose`  
`sudo ufw allow ssh`

### Security Logs

`last`  
`lastlog`  
`sudo grep -i "failed" /var/log/auth.log`

### Updates

`sudo apt update`  
`sudo apt upgrade`  
`apt list --upgradable`

---

## Real-Work Security Workflow

User / Login Problem  
↓  
`who` / `w`  
↓  
`last`  
↓  
Check `/var/log/auth.log`  
↓  
Check running services  
↓  
`sudo ss -tulpn`  
↓  
Check UFW  
↓  
Identify root cause  
↓  
Apply controlled fix  
↓  
Verify SSH / service / firewall  
↓  
Document the change

---

## Result

Successfully studied the main Linux security and SSH-hardening practices required for basic server administration.

The module covered:

- User privileges
- File permissions
- File ownership
- SSH security
- SSH configuration
- SSH key authentication
- UFW firewall
- Open ports
- Login investigation
- Authentication logs
- Security updates
- Basic security troubleshooting

## Module Status

**M15 — COMPLETED** ✅
