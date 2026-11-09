# Samba NAS Infrastructure Services

## Overview

This module documents the Samba-based network-attached storage services deployed across the infra-ops homelab.

Two Linux infrastructure nodes provide independently accessible NAS storage:

- `infra-hub` — primary NAS services
- `redundant-net` — secondary NAS services

Samba provides network file sharing, while Linux manages the underlying storage mounts and filesystems.

The two-node design provides separate storage resources. It does not, by itself, establish automated file replication or backup synchronization.

---

## Infrastructure Architecture

| Node | Storage Role | Mount Point | Approximate Capacity |
| --- | --- | --- | --- |
| `infra-hub` | Primary NAS | `/mnt/hub-nas` | 1 TB |
| `redundant-net` | Secondary NAS | `/mnt/rn-nas` | 3 TB |

Both nodes use the NTFS3 filesystem driver for their NAS volumes.

Samba provides the network-sharing layer above these mounted filesystems.

---

## Infrastructure Role

The NAS services support:

- Network-accessible file storage
- Shared file management
- Persistent storage across host reboots
- Access from authorized infrastructure clients
- Storage capacity and health monitoring
- Independent primary and secondary storage locations

File replication, automated backups, and disaster recovery require separate implementation and validation. The presence of a secondary NAS is not proof that the primary NAS data has been backed up.

---

## Samba Installation

Install the Samba service on an Ubuntu host:

```bash
sudo apt update
sudo apt install samba
```

Samba is managed through the `smbd` systemd service.

The production NAS volumes are mounted under `/mnt`; they are not created using a separate `/srv/samba/shared` directory.

---

## Share Configuration

Samba share definitions are maintained in:

```text
/etc/samba/smb.conf
```

The production configurations were inspected using `testparm -s` on November 8, 2026.

The validated share mappings are:

| Node | Share Name | Filesystem Path |
| --- | --- | --- |
| `infra-hub` | `HubNAS` | `/mnt/hub-nas` |
| `redundant-net` | `backup-share` | `/mnt/rn-nas` |

Both shares were configured for read/write access at the Samba configuration level.

### Access Control

Guest-enabled file sharing is not the recommended deployment pattern for infrastructure storage.

Share access depends on Samba authentication, filesystem permissions, and the applicable share restrictions.

An authenticated share example is shown below for illustration. It is not a copy of either production configuration:

```ini
[NASShare]
path = /mnt/your-nas-volume
browseable = yes
read only = no
guest ok = no
valid users = your-authorized-samba-account
```

Replace the illustrative path and account with configured local values before using such a share definition.

Do not assume that changing `smb.conf` alone establishes correct underlying filesystem access.

Validate ownership, permissions, Samba user provisioning, and client authentication before enabling a new share.

---

## Service Management

Check whether Samba is active:

```bash
systemctl is-active smbd
```

Inspect service details:

```bash
systemctl status smbd
```

Validate the Samba configuration:

```bash
testparm -s
```

Changes to Samba configuration should be backed up, syntax-checked, and tested before restarting the service.

---

## Storage Validation

Confirm the relevant NAS filesystem is mounted:

```bash
findmnt /mnt/hub-nas
```

On the secondary node:

```bash
findmnt /mnt/rn-nas
```

Review storage capacity:

```bash
df -h
```

Inspect filesystem information:

```bash
lsblk -f
```

Successful mounting and an active Samba service are necessary checks, but actual share accessibility and read/write operations must also be tested from an authorized client.

---

## Monitoring and Observability

NAS storage and service health are integrated into the homelab monitoring environment.

Tools include:

- Grafana
- Prometheus
- Node Exporter
- Glances
- Uptime Kuma

Monitoring provides visibility into relevant storage and host metrics, including capacity, disk usage, service status, and available drive-health information.

Dashboard screenshots are maintained in:

```text
docs/monitoring/screenshots/
```

Monitoring documentation is maintained in:

```text
docs/monitoring/README.md
```

---

## Operational Recovery

### NAS Mount Standardization

Storage mount paths were standardized across the two infrastructure nodes:

- `/mnt/hub-nas`
- `/mnt/rn-nas`

This improved consistency across service configuration, monitoring, maintenance, and documentation.

### Secondary NAS Filesystem Recovery

On October 25, 2026, the secondary NAS experienced an NTFS filesystem dirty-flag condition that interfered with normal mounting.

Recovery included:

- Investigating the filesystem condition
- Clearing the dirty flag
- Removing the temporary forced-mount configuration
- Restoring normal NTFS3 mounting
- Validating read/write access
- Confirming Samba availability

The incident and its verified recovery are documented separately:

[RN NAS Filesystem Recovery](../docs/operations/rn-nas-recovery-2026-10-25.md)

---

## Operational Principles

NAS maintenance follows these practices:

1. Verify the physical storage device and filesystem.
2. Confirm the correct mount point.
3. Validate Samba configuration syntax.
4. Confirm the Samba service is running.
5. Test share access from an authorized client.
6. Verify filesystem read/write behavior.
7. Check storage and service monitoring.
8. Document validated production changes.

Sensitive account information and private network details are excluded from public documentation.

---

## Summary

The infra-ops homelab operates Samba NAS services on both primary and secondary Linux infrastructure nodes.

The deployment includes separate NTFS3-backed storage volumes, network file sharing, systemd service management, and integrated monitoring.

Documented recovery work demonstrates troubleshooting of filesystem mount conditions, service availability, and storage accessibility.

These are independently operated NAS services; automatic replication or backup synchronization is not claimed.
