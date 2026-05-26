# Samba NAS Infrastructure Services

## Overview

This module documents the deployment and operational configuration of Samba-based network-attached storage services within the `infra-ops` infrastructure environment.

The Samba NAS service provides centralized file sharing, persistent infrastructure storage, and network-accessible data management across the local infrastructure environment.

The service operates on the primary infrastructure node:

- Hostname: `infra-hub`
- Platform: Ubuntu Server
- Role: Centralized Network Storage & Shared Infrastructure Services

---

## Infrastructure Role

The Samba NAS service functions as the centralized storage layer within the `infra-ops` environment.

Primary responsibilities include:

- Network file sharing
- Persistent infrastructure storage
- Shared operational data access
- Centralized file management
- Cross-device storage accessibility
- Future backup and archival support

This infrastructure service establishes the foundation for future backup strategies, persistent service storage, and infrastructure data management.

---

## Deployment Objectives

- Deploy centralized network storage services
- Enable secure file sharing across infrastructure devices
- Maintain persistent storage availability
- Support operational data accessibility
- Improve infrastructure storage organization
- Prepare for future backup and archival integration

---

## Samba Installation & Configuration

### Install Samba Services

Update system packages and install Samba:

```bash
sudo apt update && sudo apt install samba -y
```

---

### Create Shared Storage Directory

Create the shared storage directory structure:

```bash
sudo mkdir -p /srv/samba/shared
```

Assign appropriate ownership and permissions:

```bash
sudo chown nobody:nogroup /srv/samba/shared
sudo chmod 0775 /srv/samba/shared
```

---

### Samba Configuration

Edit the Samba configuration file:

```bash
sudo nano /etc/samba/smb.conf
```

Add the following share configuration:

```ini
[Shared]
path = /srv/samba/shared
browseable = yes
read only = no
guest ok = yes
create mask = 0775
directory mask = 0775
```

---

## Service Initialization

Restart and enable Samba services:

```bash
sudo systemctl restart smbd
sudo systemctl enable smbd
```

---

## Validation & Testing

The following validation procedures were performed:

- Verified Samba configuration syntax
- Confirmed network share accessibility
- Tested Linux client connectivity
- Validated file read/write operations
- Confirmed persistent share availability after reboot
- Verified service startup behavior
- Confirmed successful network discovery

Validate Samba configuration syntax:

```bash
testparm
```

Verify Samba service status:

```bash
systemctl status smbd
```

---

## Operational Troubleshooting

### Issue: Share Mount Failures

#### Cause

Initial mount attempts failed due to incorrect share configuration formatting and inconsistent Samba permissions.

#### Resolution

- Corrected Samba configuration syntax
- Verified directory ownership and permissions
- Restarted Samba services after configuration changes
- Validated share accessibility using Linux client systems

#### Validation

Successful network share access was confirmed after service restart and configuration correction.

---

### Issue: Inconsistent Share Accessibility

#### Cause

Network discovery inconsistencies occurred during early deployment and testing.

#### Resolution

- Verified service status
- Confirmed firewall configuration
- Revalidated Samba share configuration
- Tested connectivity across multiple client devices

#### Validation

Persistent network share accessibility was confirmed after infrastructure validation procedures.

---

## Infrastructure Integration

The Samba NAS service integrates with additional infrastructure modules within the `infra-ops` ecosystem, including:

- `pihole-setup`
- `glances-monitoring`
- `redundant-net`
- `unbound`

The storage platform provides centralized operational storage services supporting infrastructure persistence, troubleshooting, and future backup strategies.

---

## Future Improvements

Planned future enhancements include:

- Automated backup integration
- Snapshot-based storage recovery
- Expanded storage capacity
- RAID-enabled storage redundancy
- Container volume persistence
- Centralized infrastructure backups
- Offsite backup replication
- Infrastructure storage monitoring integration

---

## Operational Notes

The implementation of Samba NAS services introduced centralized persistent storage capabilities into the `infra-ops` environment.

This deployment established the foundation for future infrastructure backup strategies, operational data persistence, and scalable network storage services across the infrastructure platform.
