# Hub NAS Samba Server

## Overview

This project documents the deployment and configuration of a lightweight NAS (Network Attached Storage) solution hosted on a repurposed HP laptop running Ubuntu Server. The infrastructure node functions as a centralized storage server while also supporting network services including Pi-hole, Unbound DNS, Glances monitoring, and remote SSH administration.

The NAS was configured using Samba for Windows/Linux interoperability and persistent NTFS external storage mounting for long-term accessibility and operational stability.

---

## Objectives

* Deploy centralized NAS storage on low-resource hardware
* Configure persistent external drive mounting using UUIDs
* Enable Windows/Linux network file sharing via Samba
* Practice Linux system administration and infrastructure troubleshooting
* Maintain lightweight infrastructure suitable for a home lab environment
* Document deployment steps and operational workflows

---

## Infrastructure Overview

### Primary Infrastructure Hub

* Hostname: `blackhole`
* Device Role: Infrastructure Hub Server
* OS: Ubuntu Server
* Management Method: SSH
* IP Address: `192.168.1.226`

### Services Running

* Pi-hole
* Unbound DNS
* Glances Monitoring
* Samba NAS
* SSH Remote Administration

---

## Storage Configuration

### External NAS Storage

* Drive Type: External USB HDD
* Filesystem: NTFS
* Mount Point:

```bash
/mnt/hub_nas
```

### Persistent Mount Configuration

Configured using `/etc/fstab` with UUID-based mounting for reboot persistence.

Example:

```bash
UUID=4E1AEA7B1AEA6007 /mnt/hub_nas ntfs-3g defaults 0 0
```

---

## Samba Configuration

### Samba Share Name

```bash
HubNAS
```

### SMB Access

Accessible from Windows File Explorer using:

```bash
\\192.168.1.226\
```

### Example Samba Share Configuration

```ini
[HubNAS]
   path = /mnt/hub_nas
   browseable = yes
   writable = yes
   read only = no
   guest ok = no
```

---

## Deployment Steps

1. Connected and identified external storage using `lsblk`
2. Verified filesystem and UUID using `blkid`
3. Created Linux mount point
4. Mounted NTFS storage using `ntfs-3g`
5. Installed and configured Samba
6. Configured SMB authentication
7. Troubleshot Samba configuration syntax issues
8. Configured persistent mounting with `/etc/fstab`
9. Validated reboot persistence using `mount -a`

---

## Troubleshooting Notes

### Initial NTFS Mount Failure

Issue:

```bash
wrong fs type, bad option, bad superblock
```

Resolution:

* Verified `ntfs-3g` installation
* Switched mount type from `ntfs3` to `ntfs-3g`

### Samba Service Failure

Issue:

```bash
set_variable_helper(no#): value is not boolean!
```

Resolution:

* Located malformed Samba config entry:

```ini
guest ok = no#
```

* Removed invalid trailing character
* Validated configuration using:

```bash
testparm
```

---

## Skills Demonstrated

* Linux System Administration
* Samba Configuration
* NAS Deployment
* Persistent Storage Mounting
* SSH Remote Administration
* Infrastructure Troubleshooting
* Network File Sharing
* Home Lab Infrastructure Design
* Cross-Platform File Sharing
* Service Validation and Testing

---

## Future Improvements

* Docker Integration
* Grafana Monitoring
* Automated Backups
* VPN Remote Access
* Infrastructure Topology Diagrams
* Backup Node Integration
* Network Segmentation
* Secondary NAS Redundancy

---

## Project Status

Active / Ongoing Home Lab Infrastructure Project
