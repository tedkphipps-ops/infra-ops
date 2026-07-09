# System Maintenance

## Overview

This section documents the routine maintenance procedures used to keep the homelab healthy, secure, and operating reliably after deployment.

Unlike the installation guides throughout this repository, this documentation focuses on maintaining an existing production environment.

Current infrastructure nodes:

| Node | Hostname | Role |
|------|----------|------|
| HUB | `infra-hub` | Primary infrastructure node |
| RN | `redundant-net` | Secondary / redundant infrastructure node |

---

## Purpose

Routine maintenance includes:

- Applying operating system updates
- Verifying core infrastructure services
- Validating storage availability
- Confirming monitoring functionality
- Maintaining repository documentation that accurately reflects the production environment

---

# Routine Health Check

The following checks provide a quick assessment of overall system health before applying updates or after significant configuration changes.

## Package Updates

```bash
sudo apt update
sudo apt list --upgradable
```

Review available operating system and security updates.

---

## Docker

```bash
sudo docker ps
```

Verify production containers are running.

Expected services include:

- Grafana
- Loki
- Promtail
- Pi-hole Exporter
- Uptime Kuma (infra-hub)

---

## Failed Services

```bash
systemctl --failed
```

Review any failed systemd services.

> **Note:** `unbound-resolvconf.service` may appear failed while `unbound.service` continues operating normally.

Always verify Unbound before treating this as a production issue.

---

## Storage

```bash
df -h
```

Verify:

- Root filesystem
- `/mnt/hub-nas`
- `/mnt/rn-nas`

Confirm expected mount points and available storage.

---

## System Status

```bash
uptime
```

Review system uptime and load averages.

---

## DNS

```bash
sudo systemctl status unbound
```

Expected status:

```
Active: active (running)
```

---

## Running Services

```bash
sudo systemctl --type=service --state=running
```

Verify core production services are operational, including:

- Docker
- Pi-hole
- Unbound
- Prometheus
- Node Exporter
- Grafana
- Glances
- Samba
- SSH
- Chrony

---

## Time Synchronization

```bash
chronyc tracking
chronyc sources
```

Verify Chrony is synchronized with healthy upstream time sources.

---

## System Information

```bash
hostnamectl
```

Confirm:

- Hostname
- Ubuntu version
- Kernel version
- Hardware platform

---

# Applying Updates

After completing the health check:

```bash
sudo apt upgrade
```

When updates complete:

```bash
sudo apt autoremove
sudo apt autoclean
```

Reboot if required:

```bash
sudo reboot
```

---

# Post-Reboot Validation

After reconnecting, verify:

```bash
hostnamectl
df -h
sudo docker ps
systemctl --failed
chronyc tracking
```

Confirm:

- Updated kernel (if applicable)
- Docker containers restarted successfully
- NAS storage mounted
- Core services operational
- Chrony synchronized

---

# Storage Validation

Inspect available storage:

```bash
lsblk -f
```

Review automatic mount configuration:

```bash
cat /etc/fstab
```

Apply configured mounts:

```bash
sudo mount -a
```

Verify mounted storage:

```bash
df -h
mount | grep nas
```

Expected mount points:

| Node | Mount Point |
|------|-------------|
| `infra-hub` | `/mnt/hub-nas` |
| `redundant-net` | `/mnt/rn-nas` |

---

# Known Maintenance Notes

## RN NAS Mount Validation

During routine maintenance, the RN NAS mount was found to be unavailable.

Validation confirmed:

- External drive detected
- NTFS filesystem healthy
- UUID matched `/etc/fstab`
- Mount configuration correct
- `sudo mount -a` restored the mount
- Automatic mounting confirmed after reboot

This event has not recurred since validation.

---

# Documentation Policy

Repository documentation is maintained as a reflection of the current production environment.

After production changes are verified:

- Update relevant documentation
- Capture new screenshots when appropriate
- Update architecture diagrams when necessary
- Use placeholder values instead of exposing private IP addresses

Example:

```
<infra-hub-ip>
<redundant-net-ip>
```

---

# Maintenance Checklist

## Routine Health Check

```text
sudo apt update
sudo apt list --upgradable
sudo docker ps
systemctl --failed
df -h
uptime
sudo systemctl status unbound
sudo systemctl --type=service --state=running
chronyc tracking
chronyc sources
hostnamectl
```

## Update Procedure

```text
sudo apt upgrade
sudo apt autoremove
sudo apt autoclean
sudo reboot
```

## Post-Reboot Validation

```text
hostnamectl
df -h
sudo docker ps
systemctl --failed
chronyc tracking
```

---

# Current Production Baseline

Current production baseline:

- Ubuntu 26.04 LTS
- Linux kernel 7.0.0-27
- Docker healthy
- Pi-hole operational
- Unbound operational
- Prometheus operational
- Grafana operational
- Loki operational
- Promtail operational
- Glances operational
- Samba operational
- Chrony synchronized
- HUB NAS mounted
- RN NAS mounted

This document will evolve alongside the production environment as additional maintenance procedures are established and validated.
