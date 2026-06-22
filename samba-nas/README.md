# Samba NAS Infrastructure Services

## Overview

This module documents the deployment, configuration, and operational integration of Samba-based Network Attached Storage (NAS) services within the `infra-ops` infrastructure environment.

The Samba NAS platform provides centralized file sharing, persistent infrastructure storage, network-accessible data management, and storage redundancy support across the homelab ecosystem.

Storage services currently operate across both infrastructure nodes:

* `infra-hub` – Primary NAS Services
* `redundant-net` – Backup NAS Services

Together these systems provide storage availability, operational persistence, infrastructure data management, and future backup capabilities.

---

## Infrastructure Role

The Samba NAS platform functions as the storage layer within the `infra-ops` environment.

Primary responsibilities include:

* Network file sharing
* Persistent infrastructure storage
* Shared operational data access
* Centralized file management
* Cross-device storage accessibility
* Backup storage services
* Future disaster recovery support
* Infrastructure data retention

The NAS platform serves as a foundational component supporting infrastructure operations, monitoring, documentation, backups, and long-term scalability.

---

## Infrastructure Architecture

| Node            | Role                 | Storage Mount  |
| --------------- | -------------------- | -------------- |
| `infra-hub`     | Primary NAS Services | `/mnt/hub-nas` |
| `redundant-net` | Backup NAS Services  | `/mnt/rn-nas`  |

Current storage deployment:

* HUB NAS Capacity: ~1 TB
* RN NAS Capacity: ~3 TB
* NTFS3 storage driver standardized across both systems
* Samba services available through both infrastructure nodes

---

## Deployment Objectives

* Deploy centralized network storage services
* Enable secure file sharing across infrastructure devices
* Maintain persistent storage availability
* Support operational data accessibility
* Improve infrastructure storage organization
* Establish backup storage capabilities
* Prepare for future disaster recovery workflows
* Support long-term infrastructure scalability

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

Example share configuration:

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

* Verified Samba configuration syntax
* Confirmed network share accessibility
* Tested Linux client connectivity
* Validated file read/write operations
* Confirmed persistent share availability after reboot
* Verified service startup behavior
* Confirmed successful network discovery
* Verified NAS mount accessibility
* Confirmed storage visibility through monitoring platforms

Validate Samba configuration:

```bash
testparm
```

Verify Samba service status:

```bash
systemctl status smbd
```

Verify mounted storage:

```bash
df -h
```

---

## Monitoring & Visibility

NAS services are monitored through:

* Grafana dashboards
* Prometheus
* Node Exporter
* Glances
* Uptime Kuma

Current monitoring includes:

* NAS capacity utilization
* Used storage
* Free storage
* NAS service status
* Disk health status
* Drive temperature monitoring

Monitoring screenshots are maintained within:

```text
docs/monitoring/screenshots/
```

---

## Operational Troubleshooting

### NAS Mount Standardization

Infrastructure storage paths were standardized to improve consistency and documentation alignment.

Current standards:

* `/mnt/hub-nas`
* `/mnt/rn-nas`

This standardization simplified monitoring, documentation, dashboard management, and future infrastructure growth.

---

### Storage Monitoring Integration

NAS visibility was expanded through integration with Node Exporter, Prometheus, and Grafana.

This deployment enabled:

* Capacity monitoring
* Utilization tracking
* Health monitoring
* Storage observability across both infrastructure nodes

---

## Infrastructure Integration

The Samba NAS platform integrates with additional infrastructure modules throughout the `infra-ops` ecosystem:

* `pihole-setup`
* `unbound`
* `glances-monitoring`
* `monitoring`
* `redundant-net`

The NAS platform supports infrastructure persistence, documentation storage, monitoring data retention, backup workflows, and future disaster recovery objectives.

---

## Future Improvements

Planned future enhancements include:

* Automated backup workflows
* Cross-node backup synchronization
* Disaster recovery testing
* Expanded storage capacity
* Future RAID research and evaluation
* Additional storage monitoring
* Long-term backup retention policies
* Infrastructure storage analytics

---

## Operational Notes

The implementation of Samba NAS services introduced centralized storage capabilities into the homelab environment and established the foundation for infrastructure persistence and backup operations.

The storage platform evolved from a single-node file share into a dual-node NAS architecture supporting both primary and backup storage services.

A major operational milestone included NAS mount standardization across infrastructure nodes, migration to NTFS3 storage drivers, and integration with the centralized monitoring stack.

The Samba NAS platform remains a core infrastructure service supporting operational continuity, documentation management, monitoring visibility, and future disaster recovery planning across the homelab ecosystem.
