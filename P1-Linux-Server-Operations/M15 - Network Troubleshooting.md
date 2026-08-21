# M15 — Network Troubleshooting

## Objective

Learn how to systematically identify, investigate, troubleshoot, and verify Linux network connectivity problems.

## Lab Environment

- Ubuntu Server
- Oracle VirtualBox
- Linux Server Home Lab
- Windows Host → Ubuntu Server

---

## 1. Check Network Interface

### Commands

`ip -br link`  
`ip link`  
`ip addr`

### Check

- Interface name
- Interface state
- IP address
- Network interface errors

---

## 2. Check IP Configuration

### Commands

`ip addr show`  
`ip route`  
`ip -br addr`

### Check

- IP address
- Subnet
- Default route
- Network interface

---

## 3. Check Default Gateway

### Command

`ip route`

### Example

`ip route | grep default`

### Key Point

If the default route is missing or incorrect, the server may not be able to reach networks outside its local subnet.

---

## 4. Test Local Network Connectivity

### Command

`ping <gateway-ip>`

### Example

`ping 192.168.176.1`

### Purpose

Tests connectivity between the Ubuntu Server and its local gateway.

---

## 5. Test Internet Connectivity

### Commands

`ping 8.8.8.8`  
`ping 1.1.1.1`

### Key Point

If IP addresses can be reached but domain names cannot, investigate DNS.

---

## 6. Test DNS Resolution

### Commands

`resolvectl status`  
`resolvectl query google.com`  
`getent hosts google.com`

### Key Point

DNS problems can prevent applications from reaching services by hostname even when network connectivity is working.

---

## 7. Test Connectivity by Hostname

### Commands

`ping google.com`  
`getent hosts google.com`

### Troubleshooting Logic

`ping 8.8.8.8` works  
↓  
`ping google.com` fails  
↓  
Investigate DNS

---

## 8. Check Listening Ports

### Command

`sudo ss -tulpn`

### Check Specific Port

`sudo ss -tulpn | grep :22`

### Key Point

Useful when a service appears to be running but clients cannot connect.

---

## 9. Test a TCP Port

### Commands

`nc -zv <IP> <port>`  
`nc -zv <hostname> <port>`

### Example

`nc -zv 192.168.176.128 22`

### Key Point

Tests whether a TCP port is reachable.

---

## 10. Trace Network Path

### Commands

`tracepath 8.8.8.8`  
`traceroute 8.8.8.8`

### If traceroute is not installed

`sudo apt install traceroute`

### Key Point

Useful for identifying where packets stop or experience routing problems.

---

## 11. Check ARP / Neighbor Information

### Command

`ip neigh`

### Key Point

Useful for investigating local-network connectivity and neighbour/ARP resolution.

---

## 12. Check Network Interface Statistics

### Command

`ip -s link`

### Check

- RX errors
- TX errors
- Dropped packets
- Packet counts

### Key Point

Increasing errors or drops may indicate interface, driver, cable, or network problems.

---

## 13. Check Network Configuration

### Commands

`networkctl status`  
`resolvectl status`

### Key Point

Use these commands to understand interface state and DNS configuration on systems using systemd networking components.

---

## 14. Network Troubleshooting Scenario — No Internet

### Step 1 — Check Interface

`ip -br link`

### Step 2 — Check IP

`ip -br addr`

### Step 3 — Check Route

`ip route`

### Step 4 — Ping Gateway

`ping <gateway-ip>`

### Step 5 — Ping External IP

`ping 8.8.8.8`

### Step 6 — Test DNS

`resolvectl query google.com`

### Step 7 — Check DNS Configuration

`resolvectl status`

### Step 8 — Verify

`ping google.com`

---

## 15. Network Troubleshooting Scenario — SSH Connection Failure

### From Windows Host

Test the Ubuntu Server IP:

`ping <ubuntu-ip>`

### On Ubuntu

`ip -br addr`

Check SSH:

`systemctl status ssh`

Check port:

`sudo ss -tulpn | grep :22`

### Test TCP Port

`nc -zv <ubuntu-ip> 22`

### Check Firewall

`sudo ufw status`

### Verify

`ssh <user>@<ubuntu-ip>`

---

## 16. Network Troubleshooting Scenario — DNS Failure

### Test IP Connectivity

`ping 8.8.8.8`

### Test DNS

`resolvectl query google.com`

### Check DNS

`resolvectl status`

### Test Hostname

`ping google.com`

### Troubleshooting Logic

IP connectivity works  
+  
DNS resolution fails  
=  
Investigate DNS configuration/service.

---

## 17. Network Troubleshooting Scenario — Service Port Unreachable

### Check Service

`systemctl status <service>`

### Check Listening Port

`sudo ss -tulpn | grep :<port>`

### Check Firewall

`sudo ufw status`

### Test Port

`nc -zv <server-ip> <port>`

### Verify

Confirm that:

- Service is running.
- Service is listening on the expected port.
- Firewall allows the required traffic.
- Client can reach the server.

---

## 18. VirtualBox NAT → Bridged Troubleshooting

### Check Ubuntu IP

`ip -br addr`

### Check Route

`ip route`

### Test Gateway

`ping <gateway-ip>`

### Test Internet

`ping 8.8.8.8`

### From Windows

`ping <ubuntu-ip>`

### Verify SSH

`ssh <user>@<ubuntu-ip>`

### Key Point

When changing VirtualBox networking from NAT to Bridged Adapter, always re-check:

- IP address
- Default gateway
- Connectivity
- SSH access

---

## 19. Network Troubleshooting Workflow

### Problem

Network connectivity failure

↓

`ip -br link`

↓

`ip -br addr`

↓

`ip route`

↓

Ping local gateway

↓

`ping <gateway-ip>`

↓

Ping external IP

↓

`ping 8.8.8.8`

↓

Test DNS

↓

`resolvectl query google.com`

↓

Check ports

↓

`sudo ss -tulpn`

↓

Check firewall

↓

`sudo ufw status`

↓

Identify Root Cause

↓

Apply Fix

↓

Verify Connectivity

---

## 20. Important Commands Summary

### Interface

`ip -br link`  
`ip link`  
`ip addr`

### IP / Routing

`ip -br addr`  
`ip route`

### Connectivity

`ping <gateway-ip>`  
`ping 8.8.8.8`  
`ping google.com`

### DNS

`resolvectl status`  
`resolvectl query google.com`  
`getent hosts google.com`

### Ports

`sudo ss -tulpn`  
`nc -zv <IP> <port>`

### Routing Path

`tracepath 8.8.8.8`  
`traceroute 8.8.8.8`

### Neighbours

`ip neigh`

### Interface Statistics

`ip -s link`

### Firewall

`sudo ufw status`

---

## Result

Successfully studied the main Linux network troubleshooting techniques required for server operations.

The module covered:

- Network interface troubleshooting
- IP configuration
- Default gateway
- Routing
- Local connectivity
- Internet connectivity
- DNS troubleshooting
- Port connectivity
- Network interface errors
- ARP / neighbour troubleshooting
- VirtualBox NAT → Bridged troubleshooting
- Windows → Ubuntu connectivity
- SSH connectivity troubleshooting
- Structured network troubleshooting workflow

## Module Status

**M15 — COMPLETED** ✅
