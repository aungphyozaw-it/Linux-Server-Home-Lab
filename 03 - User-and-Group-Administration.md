# M03 — User & Group Administration  

## Objective  
Manage Linux users and groups on Ubuntu Server and practise basic account administration required for server operations. 
The objective was to create and manage user accounts, inspect user and group information, configure administrative privileges and verify access using standard Linux commands. 


## Lab Environment  
- **Server OS:** Ubuntu Server 
- **Virtualization:** Oracle VirtualBox 
- **Lab Type:** Linux Server Home Lab 
- **Primary Administrative User:** aung 
- **Additional Lab Users:** `developer1`, developer3

 
## Scenario 
A Linux server requires multiple user accounts for administration and application-related activities.  User accounts and group memberships must be managed correctly so that users receive appropriate access and administrative privileges.  The lab was used to practise user creation, user identification, group membership and sudo administration.  


## Expected Behaviour  
- Linux users can be identified and managed.
- User and group information can be inspected.
- Users can be assigned to appropriate groups.
- Administrative privileges can be granted through the `sudo` group.
- User access can be verified after configuration changes.
- User and group administration can be performed using standard Linux commands.

 
## User Information  
### Check Current User  
   `whoami`
**Purpose:** Display the username of the currently logged-in user. The lab environment used the `aung` user for normal administrative activities.

 
### Display User and Group IDs
 `id`  
**Purpose:** Display the current user's UID, GID and supplementary group memberships.

 
### Check a Specific User
 `id developer1`  
**Purpose:** Display UID, GID and group membership information for the `developer1` user.


## User and Home Directory Verification
### List Home Directories
 `ls /home`  
**Purpose:** Verify the user home directories available on the server.
The lab environment included user directories such as:
- `/home/aung`
- `/home/developer1`

 
## User Administration
Linux user accounts were managed using standard account administration commands.

 
### Create a User
`sudo useradd -m <username>`  
**Purpose:** Create a new user account and create its home directory.

 
### Set a User Password
`sudo passwd <username>`  
**Purpose:** Configure or change the password for a Linux user.

 
### Switch User
 `su - <username>`  
**Purpose:** Switch to another Linux user account for access testing.

 
## Group Administration
Groups were used to organise user permissions and administrative access.

 
### Display Current Group Membership
 `groups`  
**Purpose:** Display the groups associated with the current user.

 
### Display Groups for a Specific User
 `groups developer1`  
**Purpose:** Check the group memberships assigned to `developer1`.

 
### Add a User to the sudo Group
`sudo usermod -aG sudo developer3`  
**Purpose:** Add `developer3` to the `sudo` group so the user can perform authorised administrative operations.
The `-aG` options ensure that the user is added to the supplementary group without removing existing supplementary group memberships.

 
## Verify sudo Group Membership
After adding the user to the `sudo` group, membership was verified.
 `id developer3`  
or:
 `groups developer3`  
**Purpose:** Confirm that `developer3` has been added to the expected administrative group.

 
## Permission / Access Verification 
User and group information was checked after changes to verify that the expected account configuration was applied.
Verification included:
`id developer3`  `groups developer3`  
The results were used to confirm the user's group membership and administrative access configuration.

 
## Troubleshooting Methodology
User and group access issues were investigated systematically.
The troubleshooting process was:
**Identify User → Check UID/GID → Check Groups → Check Administrative Membership → Apply Fix → Verify**
Useful commands included:
 `whoami`  `id  id` `developer3`  `groups developer3`

 
## Root Cause Analysis
When a user does not have the expected administrative access, the user's group membership must be checked before changing permissions or other system configuration.
 The investigation focused on:
- User identity
- UID and GID
- Supplementary groups
- `sudo` group membership
- User account configuration

 
## Fix / Recovery
Where administrative access was required, the appropriate user was added to the `sudo` group.
Example:
`sudo usermod -aG sudo developer3`  
The resulting group membership was then verified using:
 `id developer3`  
or:
 `groups developer3`

 
## Verification
The following checks were completed:
- Current user identified with `whoami`.
- User identity and group information checked with `id`.
- Home directories verified.
- User accounts inspected.
- Group memberships checked.
- `developer3` was added to the sudo group.
- Updated group membership was verified.

  
## Result 
Linux user and group administration was successfully practised in the Ubuntu Server lab. 
User identities, group memberships and administrative privileges were inspected and configured using standard Linux administration commands.
The `developer3` account was added to the `sudo` group and the resulting membership was verified.


## Evidence
Evidence for this module includes:
- `whoami` output
- `id` output
- `ls /home` output
- User account information
- Group membership information
- `id developer3` output
- `groups developer3` output
- `sudo` group membership verification

 
## Skills Demonstrated
- Linux User Administration
- Linux Group Administration
- User Identity Management
- UID / GID Verification
- Group Membership Management
- sudo Administration
- Linux Access Control Fundamentals
- User Troubleshooting
- Command-Line Administration

 
## Lessons Learned
- How to identify the currently logged-in Linux user.
- How to inspect UID, GID and group membership.
- How Linux users and groups are used to control access.
- How to inspect user home directories.
- How to add a user to the sudo group.
- Why group membership should be verified after making account changes.
- How user and group information can help diagnose access-related problems.

 
## Module Status
 
**M03 — COMPLETED**
