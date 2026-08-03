# SSH Configuration

## Objective

Configure SSH on Ubuntu Server and verify remote access from a Windows client.

## Lab Environment

- Oracle VirtualBox
- Ubuntu Server
- Windows 11 Client
  

## Commands Used

### Check whether OpenSSH Server is installed

```bash
dpkg -l | grep openssh
```

**Purpose:** Verify that the OpenSSH Server package is installed.

---

### Check SSH service status

```bash
systemctl status ssh
```

**Purpose:** Check whether the SSH service is running.

---

### Start the SSH service

```bash
sudo systemctl start ssh
```

**Purpose:** Start the SSH service.

---

### Enable SSH at boot

```bash
sudo systemctl enable ssh
```

**Purpose:** Ensure the SSH service starts automatically after system boot.

---

### Check the server IP address

```bash
ip addr
```

**Purpose:** Display the server IP address for remote SSH access.

---

### Connect from Windows

```bash
ssh aung@<Ubuntu-IP-Address>
```

**Purpose:** Connect remotely from the Windows client to the Ubuntu Server.



## Result

- Successfully configured SSH on Ubuntu Server.
- Verified the SSH service was running.
- Connected to the Ubuntu Server remotely from Windows using SSH.

## Skills Gained

- SSH Configuration
- Linux Service Management
- Remote Server Administration
- Basic Network Connectivity
