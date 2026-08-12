# M001 — File & Directory Operations

## Objective

Practise basic Linux file and directory operations on Ubuntu Server and understand how files, directories and permissions are managed from the command line.

## Lab Environment

- **Server OS:** Ubuntu Server
- **Virtualization:** Oracle VirtualBox
- **Lab Type:** Linux Server Home Lab
- **User:** aung

## Scenario

As part of Linux server administration, files and directories need to be created, inspected and managed from the command line.

The lab was used to practise working with user home directories, creating a working directory, creating a log file and modifying file permissions.

## File and Directory Verification

### List User Home Directories

`ls /home`

Purpose: Display the user home directories available on the Ubuntu Server.

The lab environment included users such as:

-`aung`

-`developer1`


## Create a Working Directory

A working directory was created inside the user's home directory.

`mkdir /home/aung/lap`

Purpose: Create the `lap` directory for practising file and directory operations.


## Create a Log File

A log file was created inside the working directory.

`touch /home/aung/lap/app.log`

**Purpose:** Create an empty `app.log` file for file-management practice.


## Modify File Permissions

File permissions were modified using `chmod`.

`chmod`

**Purpose:** Practise changing Linux file permissions.

The permission configuration was checked as part of the file-management exercise.


## Lab Practice

The following activities were completed during the lab:

1. Inspected the `/home` directory.


2. Identified user home directories.


3. Created the `/home/aung/lap` working directory.


4. Created the `app.log` file.


5. Practised modifying file permissions using `chmod`.



## Troubleshooting Methodology

File and directory access was investigated by checking the location of the files and directories and reviewing the permissions applied to them.

The general approach used was:

**Identify → Inspect → Modify → Verify**


## Verification

The file and directory operations were verified during the lab.

The following were successfully created and managed:

`/home/aung/lap/home/aung/lap/app.log`

File permissions were also modified using `chmod`.

## Result

Basic Linux file and directory operations were successfully practised on Ubuntu Server.

The lab demonstrated the creation and management of directories and files within the user's home directory, together with basic Linux permission management.


## Evidence

Evidence for this module includes:

- `/home` directory listing

- User home directories

- `/home/aung/lap` directory

- `app.log` file

- `chmod` permission exercise

- Command outputs from the Ubuntu Server lab


## Skills Demonstrated

- Linux File Management

- Linux Directory Management

- Command-Line Administration

- Home Directory Management

- Basic File Permissions

- `chmod`

- Linux Server Administration Fundamentals


## Lessons Learned

- How to inspect Linux user home directories.

- How to create directories on Ubuntu Server.

- How to create files from the command line.

- How Linux file permissions can be modified using `chmod`.

- How basic file and directory operations form part of everyday Linux server administration.


## Module Status

**M01 — COMPLETED**
