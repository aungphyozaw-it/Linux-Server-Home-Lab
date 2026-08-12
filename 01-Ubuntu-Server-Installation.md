# M01 — Ubuntu Server Installation & Lab Setup

## Objective

Install and prepare an Ubuntu Server virtual machine as the foundation for the Linux Server Operations & Troubleshooting Home Lab.

The objective was to build a practical Linux server environment that could be used for subsequent administration, networking, SSH, service management, storage, monitoring and troubleshooting exercises.

## Lab Environment

- **Host OS:** Windows
- **Virtualization Platform:** VirtualBox
- **Server OS:** Ubuntu Server
- **Lab Type:** Home Lab / Virtual Lab
- **Network Modes Practised:** NAT and Bridged Adapter
- **Primary Access:** Local Console and SSH
- **Server RAM:** 3 GB allocated to the VM
- **Virtual Disk:** 30 GB
- **Filesystem:** ext4

## Scenario

A Linux Server environment was required for hands-on Infrastructure Operations and Data Center administration practice.

An Ubuntu Server virtual machine was installed and configured as the primary Linux server used throughout the home lab.

The VM was later configured for network connectivity and remote administration from the Windows host.

## Expected Behaviour

After completing the initial setup:

- Ubuntu Server should boot successfully.
- The server should have a functional filesystem.
- The VM should have sufficient allocated memory and storage.
- The server should obtain network connectivity.
- The server should be able to reach external network resources.
- The environment should be ready for SSH and further Linux administration exercises.

## Virtual Machine Configuration

The Ubuntu Server VM was configured with:

- **Memory:** 3 GB
- **Storage:** 30 GB virtual disk
- **Filesystem:** ext4

A VM clone was also created for lab practice and testing.

## Network Configuration

The VM initially used **NAT** networking.

Network connectivity was verified by testing external reachability.

Example test:

`ping 8.8.8.8`

Successful replies confirmed that the Ubuntu Server had working outbound network connectivity.

The VM was later changed to **Bridged Adapter** networking to allow direct network communication between the Windows host and Ubuntu Server for SSH administration.

## System Verification

## Check System Memory

`free -h`

**Purpose:** Verify the memory available to the Ubuntu Server.

## Check Filesystem Usage

`df -h`

**Purpose:** Verify filesystem capacity and available disk space.

## Test Network Connectivity

`ping 8.8.8.8`

**Purpose:** Verify outbound network connectivity.

## Verification Results

The Ubuntu Server environment was successfully prepared for subsequent lab exercises.

The system showed:

- Approximately **2.9** GiB of available memory from the 3 GB VM allocation.
- Approximately **30 GB** filesystem capacity.
- Approximately **22 GB** available disk space during the lab verification.
- Working external network connectivity through the NAT configuration.
- Bridged networking was subsequently configured for remote SSH access.

## Evidence

Evidence for this module includes:

- Ubuntu Server VM configuration
- Virtual machine memory allocation
- Virtual disk configuration
- `free -h` output
- `df -h` output
- Network connectivity test results
- NAT networking configuration
- Bridged Adapter configuration

## Skills Demonstrated

- Ubuntu Server Installation
- Virtual Machine Configuration
- Virtual Disk Management
- Basic Filesystem Verification
- Linux Memory Verification
- Network Connectivity Testing
- NAT Networking
- Bridged Networking
- Home Lab Environment Preparation

## Lessons Learned

- How to prepare an Ubuntu Server VM for infrastructure lab work.
- How to verify Linux memory and filesystem resources.
- How to test basic network connectivity from a Linux server.
- The difference between NAT and Bridged Adapter networking in a virtual lab.
- Why network mode selection is important for remote server administration.
- How to prepare a consistent Linux environment for further troubleshooting and administration exercises.

## Module Status

**M01 — COMPLETED**
