---
id: Linux_File_System
aliases:
  - File System
tags: []
---

# File System

## Basics

### FHS

https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html

FHS: File system Hierarchy Standard

### What each file system Hold

- /bin: Essential Command Binaries used across system like bash, cat, etc
- /sbin: Essential System Binaries useful for mostly system admin accounts
- /boot: Static Files for boot loader
- /cdrom: Legacy Mounting Point for older Compact Disc Drives
- /dev: Device Files where all the virtual and hardware devices are placed/mounted
- /etc: Host-specific system-wide Configurations like apt, Bluetooth, etc
- /lib: Essential Shares Libraries and Kernel Modules
- /lib32, /lib64: Alternate format essential shared libraries
- /mnt: Mount Point for Mounting a filesystem temporarily
- /media: Mount Point for Removable Media
- /opt: Add-on Application Software Packages
- /proc:
- /root: Home Directories for the root user
- /run: Data relevant to running processes
- /snap: Snapd Packages
- /srv: Data for services provided by this system
- /sys:
- /tmp: Temporary Files
- /usr: Secondary Hierarchy
- /var: Variable Data
- /home: User Home Directories

## Resources

- [https://youtu.be/HbgzrKJvDRw?si=tcjstP0dKomPCBzP](https://youtu.be/HbgzrKJvDRw?si=tcjstP0dKomPCBzP)
- [https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
