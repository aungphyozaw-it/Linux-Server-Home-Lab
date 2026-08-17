# M05 — Networking Configuration
 
## Objective
 
Configure, inspect, and troubleshoot basic Ubuntu Server networking and verify connectivity between Ubuntu Server and a Windows client.
 
## Lab Environment
 
 
- Oracle VirtualBox
 
- Ubuntu Server
 
- Windows 11 Client
 

  
# 1. Network Interface Configuration
 
### Commands
 `ip addr ip -br addr ip link`  
### Purpose
 
 
- `ip addr` — Display interfaces and IP addresses.
 
- `ip -br addr` — Display network information in a concise format.
 
- `ip link` — Check network interface/link status.
 

  
# 2. IP Address
 
### Commands
 `ip addr hostname -I`  
### Purpose
 
 
- `ip addr` — Display detailed IP address information.
 
- `hostname -I` — Display assigned IP addresses.
 

 
### Lab Practice
 
Ubuntu Server initially received:
 `10.0.2.15/24`  
while using VirtualBox NAT networking.
  
# 3. Network Information
 
### Commands
 `ip addr ip -br addr ip link hostname hostnamectl`  
### Purpose
 
 
- `ip addr` — Interface and IP information.
 
- `ip -br addr` — Short interface/IP information.
 
- `ip link` — Interface state.
 
- `hostname` — Display hostname.
 
- `hostnamectl` — Display system/hostname information.
 

  
# 4. Connectivity Testing
 
### Commands
 `ping 8.8.8.8 ping <IP-Address> ping <Gateway-IP>`  
### Purpose
 
 
- `ping 8.8.8.8` — Test external IP connectivity.
 
- `ping <IP-Address>` — Test connectivity to another host.
 
- `ping <Gateway-IP>` — Test connectivity to the local gateway.
 

 
### Lab Practice
 
Successfully tested Internet connectivity:
 `ping 8.8.8.8`   
# 5. Default Route / Gateway
 
### Commands
 `ip route ip route get 8.8.8.8`  
### Purpose
 
 
- `ip route` — Display routing table and default gateway.
 
- `ip route get 8.8.8.8` — Show the route Linux will use to reach a destination.
 

 
### Key Concept
 `Default Route      ↓ Default Gateway      ↓ External Network`   
# 6. DNS
 
### Commands
 `ping google.com getent hosts google.com resolvectl status resolvectl query google.com`  
### Purpose
 
 
- `ping google.com` — Test connectivity and DNS resolution.
 
- `getent hosts google.com` — Test hostname resolution.
 
- `resolvectl status` — Display DNS configuration.
 
- `resolvectl query google.com` — Query DNS resolution.
 

 
### Key Concept
 `Hostname    ↓ DNS    ↓ IP Address`   
# 7. NAT → Bridged Adapter
 
### VirtualBox Configuration
 
Initial configuration:
 `NAT  ↓ Ubuntu Server  ↓ 10.0.2.15/24`  
For Windows-to-Ubuntu connectivity, the VirtualBox network adapter was changed:
 `NAT  ↓ Bridged Adapter`  
### Purpose
 
Bridged Adapter allows the Ubuntu Server VM to participate directly on the physical network.
  
# 8. Ubuntu Server IP Verification
 
### Commands
 `ip addr hostname -I ip route`  
### Purpose
 
 
- `ip addr` — Check interface/IP configuration.
 
- `hostname -I` — Quickly display server IP.
 
- `ip route` — Check gateway and routing.
 

 
### Lab Practice
 
After changing the VirtualBox adapter, the Ubuntu Server IP was checked before testing remote connectivity.
  
# 9. Windows → Ubuntu Network Connectivity
 
### Windows Commands
 `ipconfig ping <Ubuntu-IP> tracert <Ubuntu-IP>`  
### Purpose
 
 
- `ipconfig` — Display Windows network configuration.
 
- `ping <Ubuntu-IP>` — Test Windows → Ubuntu connectivity.
 
- `tracert <Ubuntu-IP>` — Trace the network path.
 

 
### Lab Practice
 
The Windows client was used to verify network connectivity to the Ubuntu Server after changing VirtualBox from NAT to Bridged Adapter.
  
# 10. Netplan Network Configuration
 
### Commands
 `ls -la /etc/netplan/ cat /etc/netplan/<config-file>.yaml sudo netplan try sudo netplan apply`  
### Purpose
 
 
- `ls -la /etc/netplan/` — List Netplan configuration files.
 
- `cat /etc/netplan/<config-file>.yaml` — View network configuration.
 
- `sudo netplan try` — Test configuration.
 
- `sudo netplan apply` — Apply configuration.
 

 
### Key Concepts
 
 
- DHCP
 
- Static IP
 
- Gateway
 
- DNS
 
- Network interface
 

  
# 11. Basic Network Troubleshooting
 
### Commands
 `ip link ip addr ip route ping <Gateway-IP> ping 8.8.8.8 getent hosts google.com resolvectl status`  
### Troubleshooting Order
 `Interface    ↓ IP Address    ↓ Subnet    ↓ Gateway    ↓ Routing    ↓ External Connectivity    ↓ DNS    ↓ Service/Application`      
# Command Quick Reference
 
  
 
Title
 
Commands
 
   
 
Network Interface Configuration
 
`ip addr`, `ip -br addr`, `ip link`
 
 
 
IP Address
 
`ip addr`, `hostname -I`
 
 
 
Network Information
 
`ip addr`, `ip link`, `hostname`, `hostnamectl`
 
 
 
Connectivity Testing
 
`ping`
 
 
 
Default Route / Gateway
 
`ip route`, `ip route get`
 
 
 
DNS
 
`getent`, `resolvectl`, `ping hostname`
 
 
 
Netplan
 
`ls /etc/netplan`, `cat`, `netplan try`, `netplan apply`
 
 
 
Windows Network
 
`ipconfig`, `ping`, `tracert`
 
  
  
# Result
 
Successfully completed basic Ubuntu Server networking practice.
 
The lab included:
 
 
- Network interface inspection
 
- IP address verification
 
- Network information
 
- Connectivity testing
 
- Default route and gateway
 
- DNS fundamentals
 
- NAT networking
 
- NAT → Bridged Adapter configuration
 
- Ubuntu Server IP verification
 
- Windows → Ubuntu network connectivity
 

 
# Module Status
 
**M05 — COMPLETED** ✅
 
**Note:** Commands and concepts that were not previously practised in the lab are included as **revision/real-work learning items** for future practice.
