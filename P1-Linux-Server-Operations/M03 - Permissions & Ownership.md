# M03 — Permissions & Ownership

## Objective

Practise Linux file and directory permissions and understand how access is controlled through permission settings and ownership.

## Lab Environment

- **Server OS:** Ubuntu Server
- **Virtualization:** Oracle VirtualBox
- **Lab Type:** Linux Server Home Lab
- **User:** `aung`

## Scenario

Linux servers use file permissions and ownership to control access to files and directories.

During the lab, file and directory permissions were inspected and modified using standard Linux commands.

The exercise focused on understanding how permission changes affect access to files and directories.


## File and Directory Permissions
Linux permissions control access for:
- Owner
- Group
- Others
Permissions are represented by:
- `r` — Read
- `w` — Write
- `x` — Execute


## Permission Verification

File and directory permissions can be inspected using: 

`ls -l`
**Purpose:** Display files and directories together with their ownership and permission information.

## Change File Permissions

During the lab, chmod was used to modify file permissions.

`chmod <permissions> <file>`

**Purpose:** Change the permission settings of a file or directory.

The permission exercise was performed on the lab files created during the File & Directory Operations practice.

## Permission Practice

The lab included:

1. Creating and working with files and directories.


2. Inspecting file permissions.


3. Modifying permissions using `chmod`.


4. Checking the resulting permission configuration.



## Ownership

Linux files and directories have an owner and group associated with them.

Ownership information can be viewed using:

`ls -l`

The output can be used to identify:

- File owner

- File group

- Permission settings


## Troubleshooting Methodology

Permission-related access issues were investigated using a structured process:

**Identify File → Inspect Permissions → Check Ownership → Modify Permission → Verify**

The primary command used for permission management was:

`chmod`

## Root Cause Analysis

When access to a file or directory does not behave as expected, the permission and ownership configuration must be checked.

The investigation focused on:

- Read permission

- Write permission

- Execute permission

- File ownership

- Group ownership

- Permission settings for owner, group and others


## Fix / Recovery

When a permission configuration needed to be changed, chmod was used to modify the file permission settings.

`chmod <permissions> <file>`

The resulting configuration was then checked using:

`ls -l`

## Verification

The following activities were completed:

- File permissions inspected.

- Ownership information inspected.

- File permissions modified using `chmod`.

- Resulting permission settings verified.


## Result

Linux file permissions and ownership concepts were successfully practised on Ubuntu Server.

The lab demonstrated how Linux controls access to files and directories through permission settings and ownership.


## Evidence

Evidence for this module includes:

- `ls -l` permission output

- File ownership information

- Permission modification using `chmod`

- Resulting permission configuration


## Skills Demonstrated

- Linux File Permissions

- Linux Ownership

- `chmod`

- Permission Verification

- Access Control Fundamentals

- Linux Troubleshooting

- Command-Line Administration


## Lessons Learned

- How Linux permissions control access to files and directories.

- The difference between owner, group and others.

- The meaning of read, write and execute permissions.

- How to inspect permissions using `ls -l`.

- How to modify permissions using `chmod`.

- Why permissions and ownership should be checked when investigating access problems.


## Module Status

**M03 — COMPLETED** ✅
