# M02 — User & Group Administration

## Objective

Practise Linux user and group administration on Ubuntu Server and verify user identities, group memberships and administrative privileges.

## Lab Environment

- **Server OS:** Ubuntu Server
- **Virtualization:** Oracle VirtualBox
- **Lab Type:** Linux Server Home Lab
- **Primary User:** `aung`
- **Additional Users:** `developer1`, `developer3`

## User and Group Identification

### Check Current User
  `whoami`

**Purpose:** Identify the currently logged-in Linux user.

The lab user was `aung`.


## Display User and Group Information

 `id`

**Purpose:** Display the current user's UID, GID and group memberships.


## User Home Directory Verification

### List Home Directories

`ls /home`

**Purpose:** Verify the user home directories available on the Ubuntu Server.

The lab environment included:

- `/home/aung`

- `/home/developer1`


## User and Group Administration

User and group information was inspected as part of the Linux administration exercises.


### Check User Information
 `id developer1`
**Purpose:** Display the UID, GID and group membership information for developer1.

## Administrative Group Membership

During the lab, `developer3` was added to the `sudo` group.

`sudo usermod -aG sudo developer3`

**Purpose:** Add `developer3` to the `sudo` administrative group.

The `sudo` group allows authorised users to perform administrative operations using `sudo`.

## Verify Group Membership

The updated group membership was checked after the configuration change.

`id developer3`

**Purpose:** Verify the UID, GID and supplementary group memberships of `developer3`.

The `sudo` group membership was verified after the user modification.

## Troubleshooting Methodology

User and group access was investigated using a structured approach:

**Identify User → Check UID/GID → Check Groups → Apply Required Group Change → Verify**

Commands used during the investigation included:

`whoami`

`id`

`id developer1`

`id developer3`

## Root Cause Analysis

When administrative access depends on group membership, the user's current group configuration must be checked before changing system permissions.

The investigation focused on:

- Current user identity

- UID and GID

- Supplementary group membership

- `sudo` group membership


## Fix / Recovery

The required administrative group membership was applied to `developer3` using:

`sudo usermod -aG sudo developer3`

The result was then verified using:

`id developer3`


## Verification

The following activities were completed:

- Current user identified with `whoami`.

- User and group information checked with `id`.

- `/home` user directories inspected.

- `developer1` user information checked.

- `developer3` was added to the `sudo` group.

- `developer3` group membership was verified.


## Result

Linux user and group administration was successfully practised on Ubuntu Server.

User identities, UID/GID information and group memberships were inspected.

The `developer3` user was successfully added to the `sudo` group and the updated group membership was verified.


## Evidence

Evidence for this module includes:

- `whoami` output

- `id` output

- `/home` directory listing

- `id` developer1 output

- `sudo usermod -aG sudo developer3`

- `id developer3` output

- `sudo` group membership verification


## Skills Demonstrated

- Linux User Administration

- Linux Group Administration

- UID / GID Verification

- Group Membership Management

- sudo Administration

- Linux Command-Line Administration

- Basic Access Management

- User Administration Troubleshooting


## Lessons Learned

- How to identify the current Linux user.

- How to inspect UID and GID information.

- How to inspect Linux group membership.

- How to verify user home directories.

- How to add a user to the `sudo` group.

- Why group membership should be verified after making account changes.

- How user and group information helps troubleshoot access-related issues.

## Module Status

**M02 — COMPLETED** ✅
