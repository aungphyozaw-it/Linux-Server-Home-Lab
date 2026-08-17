# M06 — SSH Administration & Hardening
 
## Objective
 
Configure, verify, troubleshoot, and secure SSH remote administration on Ubuntu Server.
 
## Lab Environment
 
 
- Ubuntu Server
 
- Windows Client
 
- Oracle VirtualBox
 
- OpenSSH
 

  
# 1. SSH Installation & Package Verification
 
### Commands
 `sudo apt update sudo apt install openssh-server`  
### Verify OpenSSH Packages
 `dpkg -l | grep openssh`  
### Purpose
 
 
- Install the OpenSSH server.
 
- Verify installed OpenSSH packages.
 

 
### Lab Practice
 
The following OpenSSH packages were verified during the lab:
 `openssh-client openssh-server openssh-sftp-server`   
# 2. SSH Service Status
 
### Commands
 systemctl status ssh  systemctl is-active ssh  systemctl is-enabled ssh  
### Purpose
 
 
- `systemctl status ssh` — Check SSH service status.
 
- `systemctl is-active ssh` — Check whether SSH is currently running.
 
- `systemctl is-enabled ssh` — Check whether SSH starts automatically at boot.
 

 
### Lab Practice
 
The SSH service was troubleshooting from:
 `ssh: not found      ↓ inactive (dead)      ↓ active (running)`   
# 3. Start / Stop / Restart SSH
 
### Commands
 `sudo systemctl start ssh`  `sudo systemctl stop ssh`  `sudo systemctl restart ssh`  `sudo systemctl reload ssh`  
### Purpose
 
 
- `start` — Start SSH service.
 
- `stop` — Stop SSH service.
 
- `restart` — Restart SSH service.
 
- `reload` — Reload configuration without fully stopping the service.
 

  
# 4. Enable SSH at Boot
 
### Commands
 `sudo systemctl enable ssh  sudo systemctl enable --now ssh`  
### Purpose
 
Configure SSH to start automatically when Ubuntu Server boots.
  
# 5. SSH Configuration File
 
The main SSH server configuration file is:
 `/etc/ssh/sshd_config`  
### Commands
 `sudo nano /etc/ssh/sshd_config`  
### View Configuration
 `sudo cat /etc/ssh/sshd_config`  
### Search Configuration
 `sudo grep -v '^#' /etc/ssh/sshd_config`  
### Purpose
 
Inspect and modify SSH server configuration.
  
# 6. Validate SSH Configuration
 
Before restarting SSH after changing configuration, validate the configuration.
 
### Command
 `sudo sshd -t`  
### Purpose
 
Check the SSH server configuration for syntax errors.
 
### Important Workflow
 `Edit sshd_config        ↓ sshd -t        ↓ No errors        ↓ Restart / Reload SSH`  
Never make SSH configuration changes blindly on a remote production server.
  
# 7. SSH Server Listening Port
 
### Commands
 `sudo ss -tlnp | grep ssh  sudo ss -lntp`  
### Purpose
 
Verify whether the SSH server is listening and identify the listening port.
 
Default SSH port:
 `22`   
# 8. Root Login
 
SSH root login was practised during the lab.
 
The relevant SSH configuration option is:
 `PermitRootLogin`  
### Check Configuration
 `sudo grep -i '^PermitRootLogin' /etc/ssh/sshd_config`  
### Real-Work Security Principle
 
Direct root SSH login is generally avoided on production systems.
 
A safer approach is:
 `Normal User     ↓ sudo     ↓ Administrative Privileges`   
# 9. Password Authentication
 
Password authentication was also practised during the lab.
 
The relevant configuration option is:
 `PasswordAuthentication`  
### Check Configuration
 `sudo grep -i '^PasswordAuthentication' /etc/ssh/sshd_config`  
### Real-Work Security Principle
 
For hardened production environments, SSH key-based authentication is generally preferred over password-only authentication.
  
# 10. SSH Key-Based Authentication
 
### Generate SSH Key
 
From the client:
 `ssh-keygen -t ed25519`  
### Copy Public Key to Server
 `ssh-copy-id username@server-ip`  
### Connect Using SSH Key
 `ssh username@server-ip` 
### Purpose
 
Use cryptographic SSH keys for authentication instead of relying only on passwords.
 
### Key Files
 
Client private key:
 `~/.ssh/id_ed25519`  
Client public key:
 `~/.ssh/id_ed25519.pub`  
Server authorized keys:
 `~/.ssh/authorized_keys`   
# 11. SSH Client Connection
 
### Command
 `ssh username@server-ip`  
Example:
 `ssh aung@192.168.176.128`  
### Purpose
 
Connect from a client machine to the Ubuntu Server using SSH.
  
# 12. Windows → Ubuntu SSH
 
The Windows client was used to connect to Ubuntu Server after changing VirtualBox networking from NAT to Bridged Adapter.
 
### Windows Command
 `ssh username@Ubuntu-IP`  
Example:
 `ssh aung@192.168.176.128`  
### Connection Flow
 `Windows    ↓ Network Connectivity    ↓ Ubuntu Server IP    ↓ TCP Port 22    ↓ SSH    ↓ Authentication    ↓ Ubuntu Shell`   
# 13. SSH Permission Troubleshooting
 
A permission error was encountered during the lab:
 `Permission denied`  
### Investigation Commands
 `whoami`  `id`  `groups`  
### Purpose
 
Identify the current user and group memberships.
 
### Check SSH Directory Permissions
 `ls -ld ~/.ssh`  `ls -la ~/.ssh/`  
### Important Permissions
 
Typical SSH key permissions:
 `~/.ssh              → 700 ~/.ssh/authorized_keys → 600`  
### Fix Example
 `chmod 700 ~/.ssh chmod 600 ~/.ssh/authorized_keys`   
# 14. SSH User & Sudo Access
 
User permissions can affect administrative SSH tasks.
 
### Commands
 `whoami`  `id`  `groups`  
### Add User to sudo Group
 `sudo usermod -aG sudo username`  
Example used during the lab:
 `sudo usermod -aG sudo developer3`  
### Verify
 `groups developer3`  
or:
 `id developer3`   
# 15. SSH Troubleshooting
 
### Check Service
 `systemctl status ssh`  
### Check Listening Port
 `sudo ss -tlnp | grep :22`  
### Check Server IP
 `hostname -I`  
### Check Routing
 `ip route`  
### Test Connectivity
 `ping Ubuntu-IP`  
### Check SSH Configuration
 `sudo sshd -t`  
### Check SSH Logs
 `sudo journalctl -u ssh`  
### Follow SSH Logs Live
 `sudo journalctl -u ssh -f`   
# 16. SSH Troubleshooting Workflow
 
When SSH connection fails:
 `1. Check Ubuntu IP        ↓ 2. Check network connectivity        ↓ 3. Check SSH service        ↓ 4. Check port 22        ↓ 5. Validate sshd_config       
 ↓ 6. Check user/account        ↓ 7. Check authentication        ↓ 8. Check SSH logs`                     
### Commands
 `hostname -I ip route ping <Ubuntu-IP> systemctl status ssh sudo ss -tlnp | grep :22 sudo sshd -t sudo journalctl -u ssh`   
# 17. SSH Hardening Fundamentals
 
Important SSH security controls include:
 
### Root Login
 `PermitRootLogin`  
### Password Authentication
 `PasswordAuthentication`  
### SSH Port
 `Port`  
### Key-Based Authentication
 `PubkeyAuthentication`  
### Security Principles
 
 
- Avoid direct root SSH login.
 
- Prefer SSH key authentication.
 
- Use strong user authentication.
 
- Keep OpenSSH updated.
 
- Restrict SSH access where appropriate.
 
- Monitor SSH authentication logs.
 
- Validate configuration before restarting SSH.
 

  
# 18. Commands Summary
 
  
 
Title
 
Commands
 
   
 
SSH Installation
 
`sudo apt install openssh-server`
 
 
 
Package Verification
 
`dpkg -l | grep openssh`
 
 
 
SSH Status
 
`systemctl status ssh`
 
 
 
Start SSH
 
`sudo systemctl start ssh`
 
 
 
Stop SSH
 
`sudo systemctl stop ssh`
 
 
 
Restart SSH
 
`sudo systemctl restart ssh`
 
 
 
Enable SSH
 
`sudo systemctl enable ssh`
 
 
 
SSH Configuration
 
`sudo nano /etc/ssh/sshd_config`
 
 
 
Validate Configuration
 
`sudo sshd -t`
 
 
 
Check Port
 
`sudo ss -tlnp | grep ssh`
 
 
 
SSH Connection
 
`ssh username@server-ip`
 
 
 
Generate Key
 
`ssh-keygen -t ed25519`
 
 
 
Copy Key
 
`ssh-copy-id username@server-ip`
 
 
 
User Check
 
`whoami`, `id`, `groups`
 
 
 
SSH Permissions
 
`ls -la ~/.ssh/`
 
 
 
SSH Logs
 
`sudo journalctl -u ssh`
 
 
 
Live SSH Logs
 
`sudo journalctl -u ssh -f`
 
  
  
# 19. Lab Result
 
Successfully practised SSH administration on Ubuntu Server.
 
The lab included:
 
 
- OpenSSH installation and package verification
 
- SSH service troubleshooting
 
- SSH service management
 
- SSH configuration
 
- Root login
 
- Password authentication
 
- Windows → Ubuntu SSH connectivity
 
- SSH permission troubleshooting
 
- User and sudo access verification
 

  
# 20. Real-Work Learning
 
The following areas are important for further practice:
 
 
- SSH key-based authentication
 
- SSH hardening
 
- Root login restrictions
 
- Password authentication policies
 
- SSH configuration validation
 
- SSH log investigation
 
- SSH access control
 
- SSH troubleshooting methodology
 

  
# Module Status
 
**M06 — COMPLETED** ✅
