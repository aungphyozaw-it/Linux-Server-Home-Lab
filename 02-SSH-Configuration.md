## M02 — SSH Administration & Hardening

## Objective

Configure, administer, harden and verify SSH remote access on Ubuntu Server.

The objective was to establish reliable remote administration from a Windows client while applying SSH security controls and verifying that legitimate administrative access remained functional.

## Lab Environment

- **Server OS:** Ubuntu Server
- **Client OS:** Windows 11
- **Virtualization:** Oracle VirtualBox
- **Remote Access:** OpenSSH
- **Network:** Virtual Lab Network
- **Administrative User:** `aung`

## Scenario

- A Linux server needs to be administered remotely from a Windows workstation.

- SSH was configured on the Ubuntu Server and tested from the Windows client.

- After basic SSH access was established, the SSH daemon configuration was hardened by restricting direct root login and configuring password authentication for the authorised Linux user.

- The configuration was then validated and the SSH service was restarted and verified.

## Expected Behaviour

- OpenSSH Server is installed.
- SSH service is running.
- SSH starts automatically after system boot.
- Ubuntu Server has a reachable IP address.
- The authorised Linux user can connect remotely.
- Direct root SSH login is disabled.
- SSH password authentication works for the authorised user.
- SSH configuration contains no syntax errors.
- SSH remains available after security configuration changes.

## OpenSSH Installation Verification

### Check OpenSSH Packages

`bash dpkg -l | grep openssh`

**Purpose:** Verify that the OpenSSH packages are installed.

## SSH Service Management

### Check SSH Service Status

`systemctl status ssh`

**Purpose:** Check whether the SSH service is currently running.

### Start SSH Service

`sudo systemctl start ssh`

**Purpose:** Start the SSH service when required.

### Enable SSH at Boot

`sudo systemctl enable ssh`

**Purpose:** Ensure SSH starts automatically after system boot.

## Network Verification

### Check Server IP Address

`ip addr`

**Purpose:** Identify the Ubuntu Server IP address required for remote SSH access.

Network connectivity was verified during the lab before testing remote SSH administration.

## SSH Configuration

The SSH daemon configuration was reviewed and modified through:

`sudo nano /etc/ssh/sshd_config`

The configuration changes were made as part of the SSH hardening exercise.

## SSH Hardening

### Disable Direct Root Login

The SSH configuration was changed to prevent direct remote login using the root account.

`PermitRootLogin no`

**Purpose:** Prevent direct SSH access using the root account and require administrators to authenticate using an authorised Linux user.

### Configure Password Authentication

Password authentication was configured for authorised SSH users.

`PasswordAuthentication yes`

**Purpose:** Allow the authorised Linux user to authenticate through SSH using a password.

The combination of these settings provides:

- No direct remote root login.

- Password-based SSH access for authorised users.

- Administrative access through a controlled user account.


## SSH Configuration Validation

Before restarting the SSH service, the configuration was checked for syntax errors.

`sudo sshd -t`

**Purpose:** Validate the SSH daemon configuration before applying the changes.

A successful command execution indicates that the SSH configuration syntax is valid.

## Apply SSH Configuration

After validating the configuration, the SSH service was restarted.

`sudo systemctl restart ssh`

**Purpose:** Apply the updated SSH configuration.

## Verify SSH Service

`systemctl status ssh`

**Purpose:** Confirm that the SSH service restarted successfully and remained operational after the configuration changes.

## Verify SSH Listening State

`ss -tlnp | grep ssh`

**Purpose:** Verify that the SSH service is listening for incoming connections.

## Remote SSH Connection

From the Windows client:

`ssh aung@<Ubuntu-IP-Address>`

**Purpose:** Establish a remote SSH session using the authorised Linux user.

The remote connection was successfully established using the configured user account.

## Troubleshooting Methodology

SSH issues were investigated systematically rather than changing multiple settings without verification.

The troubleshooting sequence used during the lab was:

**Check Service → Check IP → Check SSH Configuration → Validate Configuration → Check Listening State → Test Remote Connection**

Useful commands included:

`systemctl status ssh`

`ip addr`

`sudo sshd -t`

`ss -tlnp | grep ssh`

## Root Cause Analysis

SSH connectivity and configuration issues were investigated by checking:

- SSH service state

- Network/IP configuration

- SSH daemon configuration

- Configuration syntax

- SSH listening state

- User authentication


This approach helped separate service problems, network problems and authentication/configuration problems.

## Fix / Recovery

When SSH configuration changes were required:

1. The SSH configuration was edited.


2. The configuration syntax was validated.


3. The SSH service was restarted.


4. The service status was checked.


5. The listening state was verified.


6. Remote SSH access was tested again.



Configuration validation was performed using:

`sudo sshd -t`

## Verification

The following checks were completed:

- OpenSSH packages verified.

- SSH service verified.

- SSH enabled at boot.

- Ubuntu Server IP address identified.

- SSH configuration reviewed.

- Direct root SSH login disabled.

- Password authentication configured.

- SSH daemon configuration validated.

- SSH service restarted successfully.

- SSH listening state verified.

- Remote SSH connection tested from Windows.

- Authorised user successfully accessed the Ubuntu Server remotely.


## Result

SSH remote administration was successfully configured, hardened and verified.

The Ubuntu Server could be administered remotely from the Windows client using the authorised Linux user.

Direct root SSH access was disabled while password-based authentication remained available for the authorised user.

The SSH configuration was validated before applying the changes, and the SSH service remained operational after hardening.


## Evidence

Evidence for this module includes:

- OpenSSH package verification

- `systemctl status ssh` output

- Server IP address

- `/etc/ssh/sshd_config` configuration

- `PermitRootLogin no`

- `PasswordAuthentication yes`

- `sshd -t` validation

- SSH service restart

- SSH listening state

- Windows SSH connection

- Successful remote Linux shell session


## Skills Demonstrated

- OpenSSH Administration

- SSH Configuration

- SSH Hardening

- Linux Service Management

- Remote Server Administration

- User Authentication

- Root Login Restriction

- Configuration Validation

- Network Connectivity Verification

- SSH Troubleshooting

- Linux Security Fundamentals

- Root Cause Analysis


## Lessons Learned

- How to verify OpenSSH installation.

- How to manage SSH using `systemctl`.

- How to identify the server IP address.

- How to configure SSH for remote administration.

- How to disable direct root SSH access.

- How to configure password authentication for authorised users.

- How to validate SSH configuration before restarting the service.

- How to verify that SSH is listening after configuration changes.

- How to troubleshoot SSH using a structured investigation process.

- Why SSH configuration changes must always be verified to avoid losing remote administrative access.


## Module Status

**M02 — COMPLETED**
