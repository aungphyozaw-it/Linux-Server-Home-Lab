# M04 — Package Management
 
## Objective
 
Practise basic Ubuntu package management using APT, understand package repositories, update package information, and upgrade installed packages.
 
## Lab Environment
 
 
- **Server OS:** Ubuntu Server
 
- **Virtualization:** Oracle VirtualBox
 
- **Lab Type:** Linux Server Home Lab
 

 
## Topics Covered
 
 
1. APT Package Management
 
2. Package Repository
 
3. Repository Update
 
4. Package Upgrade
 

  
# 1. APT Package Management
 
APT (Advanced Package Tool) is the package management system used on Ubuntu.
 
### Check APT Version
 `apt --version`  
**Purpose:** Verify the installed APT version.
 
### Display APT Help
 `apt --help`  
**Purpose:** Display available APT commands and basic usage information.
  
# 2. Package Repository
 
Ubuntu uses configured package repositories as sources for package information and software packages.
 
### View Repository Configuration
 `cat /etc/apt/sources.list`  
**Purpose:** View the configured APT repository configuration.
 
 
Note: Depending on the Ubuntu version, repository configuration may also be stored under `/etc/apt/sources.list.d/`.
 
 
### List Repository Configuration Files
 `ls -la /etc/apt/sources.list.d/`  
**Purpose:** View additional APT repository configuration files.
  
# 3. Repository Update
 
Before upgrading installed packages, the local package information should be refreshed from the configured repositories.
 
### Update Package Information
 `sudo apt update`  
**Purpose:** Download the latest package information from configured repositories and refresh the local package index.
 
### Review Available Upgrades
 `apt list --upgradable`  
**Purpose:** Display installed packages for which newer versions are available.
  
# 4. Package Upgrade
 
After updating the package information, available package upgrades can be installed.
 
### Upgrade Installed Packages
 `sudo apt upgrade`  
**Purpose:** Upgrade installed packages to available newer versions.
 
### Upgrade with Automatic Confirmation
 `sudo apt upgrade -y`  
**Purpose:** Perform the package upgrade without asking for confirmation.
  
## Lab Workflow
 
The package-management workflow practised in the lab was:
 `APT  ↓ Package Repository  ↓ apt update  ↓ Check Available Upgrades  ↓ apt upgrade`  
## Commands Summary
 
  
 
Command Purpose
 
   
 
`apt --version`
 
Check APT version
 
 
 
`apt --help`
 
Display APT help
 
 
 
`cat /etc/apt/sources.list`
 
View repository configuration
 
 
 
`ls -la /etc/apt/sources.list.d/`
 
View additional repository configuration
 
 
 
`sudo apt update`
 
Update package information
 
 
 
`apt list --upgradable`
 
Check available package upgrades
 
 
 
`sudo apt upgrade`
 
Upgrade installed packages
 
 
 
`sudo apt upgrade -y`
 
Upgrade without confirmation
 
  
 
## Verification
 
The package-management practice was verified by reviewing the command output from APT operations.
 
The lab demonstrated:
 
 
- APT availability
 
- Repository configuration
 
- Package information updates
 
- Available upgrade checking
 
- Installed package upgrades
 

 
## Result
 
Successfully practised Ubuntu package management using APT and learned the relationship between package repositories, package information updates, and package upgrades.
 
## Skills Demonstrated
 
 
- APT Package Management
 
- Ubuntu Package Repositories
 
- Repository Management Fundamentals
 
- Package Index Management
 
- Package Updates
 
- Package Upgrades
 
- Linux Command-Line Administration
 

 
## Lessons Learned
 
 
- APT is used for package management on Ubuntu.
 
- Package repositories provide package information and software packages.
 
- `apt update` refreshes the local package information.
 
- `apt list --upgradable` shows packages with available upgrades.
 
- `apt upgrade` installs available upgrades for installed packages.
 
- Repository and package information should be checked before performing system package upgrades.
 

 
## Module Status
 
**M04 — COMPLETED** ✅
