# Maintenance Log

This log records validated maintenance events for the homelab.

---

## 2026-10-31 RN Performance and Pi-hole Exporter Recovery

Restarted RN Pi-hole Exporter after identifying excessive memory and swap consumption.

- RN RAM utilization decreased from approximately 80% to 36%.
- Exporter process swap decreased from approximately 516 MiB to zero.
- Identified and repaired Docker-to-host Pi-hole API firewall connectivity.
- Saved and validated the firewall exception.
- Confirmed exporter metrics HTTP 200.
- Validated Pi-hole, Unbound, Samba, NAS, Docker, and monitoring services.
- Firewall persistence after reboot remains untested.

Detailed record: [RN Exporter Recovery](rn-exporter-recovery-2026-10-31.md)

---

## 2026-10-31 Remote Administration Validation

Validated remote administration through Tailscale from ROG Zephyrus to HUB and RN.

- Remote SSH access to both infrastructure nodes verified.
- Tailscale integrated with existing host firewall management.
- ROG configured with Git, GitHub CLI, and local infra-ops repository.
- Lenovo WSL remains the established Ansible control environment.

Detailed record: [Tailscale Remote Access](tailscale-remote-access.md)

---

## 2026-10-25 RN NAS Filesystem Recovery

Repaired RN NAS NTFS dirty-flag issue and restored normal NTFS3 mounting.

- Cleared filesystem dirty flag.
- Removed forced mount configuration.
- Verified read-write NTFS3 mounting and Samba access.
- Confirmed NAS mounted during October 31 health checks.
- Separate USB reset investigation remains unresolved.

Detailed record: [RN NAS Recovery](rn-nas-recovery-2026-10-25.md)

---
## 2026-10-12 Static IP Baseline for Core Nodes

### Summary

Both core homelab nodes were converted from DHCP-assigned addresses to true static local netplan addresses.

This locks the infrastructure nodes to their expected management IPs before any future DHCP migration work.

Nodes:

| Node | Hostname | Interface | Static IP | Gateway |
|---|---|---|---|---|
| HUB | `infra-hub` | `eno1` | `REDACTED_PRIVATE_IP/24` | `REDACTED_PRIVATE_IP` |
| RN | `redundant-net` | `enp5s0` | `REDACTED_PRIVATE_IP/24` | `REDACTED_PRIVATE_IP` |

### Background

The Spectrum router is still providing DHCP for the LAN.

During DHCP planning, both HUB and RN were confirmed to still be using dynamic DHCP leases even though their addresses were stable. Before moving DHCP authority away from the Spectrum router, the core infrastructure nodes were converted to static local addressing.

### RN Netplan Cleanup

Before the static conversion, `redundant-net` had duplicate/stale netplan content in `/etc/netplan/00-installer-config.yaml`.

A backup was created:

| Node | Backup |
|---|---|
| `redundant-net` | `/etc/netplan/00-installer-config.yaml.bak-before-cleanup` |

The RN netplan file was cleaned, regenerated, applied, and validated while still using DHCP before the static conversion.

### Static IP Conversion

Backups were created before static conversion:

| Node | Backup |
|---|---|
| `infra-hub` | `/etc/netplan/00-installer-config.yaml.bak-before-static` |
| `redundant-net` | `/etc/netplan/00-installer-config.yaml.bak-before-static` |

Both nodes were updated through netplan and applied using `netplan try` so the configuration would roll back automatically if connectivity failed.

### Final Network State

| Node | Static address validation | Default route validation |
|---|---|---|
| `infra-hub` | `REDACTED_PRIVATE_IP/24`, `valid_lft forever` | `default via REDACTED_PRIVATE_IP dev eno1 proto static` |
| `redundant-net` | `REDACTED_PRIVATE_IP/24`, `valid_lft forever` | `default via REDACTED_PRIVATE_IP dev enp5s0 proto static` |

### DNS State

Both nodes use the local Pi-hole pair as primary DNS resolvers:

| Node | DNS order |
|---|---|
| `infra-hub` | `REDACTED_PRIVATE_IP`, `REDACTED_PRIVATE_IP` |
| `redundant-net` | `REDACTED_PRIVATE_IP`, `REDACTED_PRIVATE_IP` |

An IPv6 DNS resolver from the Spectrum/router side was still visible through `resolvectl`. This is non-blocking for the static IPv4 baseline and should be reviewed later if strict Pi-hole-only DNS enforcement is desired.

### Lenovo Service Port Checks

Validated from Lenovo WSL against each node:

| Port | Service |
|---|---|
| `22/tcp` | SSH |
| `53/tcp` | DNS |
| `80/tcp` | Pi-hole web |
| `445/tcp` | Samba |
| `61208/tcp` | Glances |
| `9090/tcp` | Prometheus |
| `9100/tcp` | Node Exporter |
| `9617/tcp` | Pi-hole Exporter |
| `3000/tcp` | Grafana |
| `3001/tcp` | Uptime Kuma |
| `3100/tcp` | Loki |

All checked ports passed on both nodes after static conversion.

### Ansible Result

Final Ansible maintenance check completed successfully:

| Node | ok | changed | unreachable | failed |
|---|---:|---:|---:|---:|
| `infra-hub` | 10 | 0 | 0 | 0 |
| `redundant-net` | 10 | 0 | 0 | 0 |

Both nodes reported zero failed systemd units. Docker was active and expected containers were running.

### Result

The core homelab nodes now have true static local IPv4 addresses, confirmed service reachability, and clean Ansible maintenance validation. This establishes a safer baseline before any future DHCP migration work.

---

## 2026-10-10 Firewall Hardening and Kernel Reboot Validation

### Summary

Both core homelab nodes were rebooted one at a time after firewall hardening and pending kernel updates.

Nodes:

| Node | Hostname | IP |
|---|---|---|
| HUB | `infra-hub` | `REDACTED_PRIVATE_IP` |
| RN | `redundant-net` | `REDACTED_PRIVATE_IP` |

### Pre-Reboot State

Both nodes were running:

| Node | Kernel before reboot | Reboot required |
|---|---|---|
| `infra-hub` | `7.0.0-28-generic` | yes |
| `redundant-net` | `7.0.0-28-generic` | yes |

Pending reboot packages included `libc6`, `linux-base`, and newer kernel images up through `linux-image-7.0.0-38-generic`.

### Reboot Order

1. `redundant-net`
2. `infra-hub`

Nodes were rebooted one at a time so the other node remained available during validation.

### Post-Reboot State

Both nodes came back successfully on:

| Node | Kernel after reboot | Reboot required |
|---|---|---|
| `infra-hub` | `7.0.0-38-generic` | no |
| `redundant-net` | `7.0.0-38-generic` | no |

### Post-Reboot Validation

Validated after reboot:

| Check | `infra-hub` | `redundant-net` |
|---|---|---|
| SSH reachable | PASS | PASS |
| Kernel updated | PASS | PASS |
| Reboot-required cleared | PASS | PASS |
| Docker service active | PASS | PASS |
| Expected Docker containers running | PASS | PASS |
| `netfilter-persistent` active | PASS | PASS |
| `netfilter-persistent` enabled | PASS | PASS |
| Lenovo service port checks | PASS | PASS |
| Full Ansible maintenance check | PASS | PASS |

### Lenovo Service Port Checks

Validated from Lenovo WSL against each node:

| Port | Service |
|---|---|
| `22/tcp` | SSH |
| `53/tcp` | DNS |
| `80/tcp` | Pi-hole web |
| `445/tcp` | Samba |
| `61208/tcp` | Glances |
| `9090/tcp` | Prometheus |
| `9100/tcp` | Node Exporter |
| `9617/tcp` | Pi-hole Exporter |
| `3000/tcp` | Grafana |
| `3001/tcp` | Uptime Kuma |
| `3100/tcp` | Loki |

All checked ports passed on both nodes.

### Ansible Result

Final post-reboot Ansible maintenance check completed successfully:

| Node | ok | changed | unreachable | failed |
|---|---:|---:|---:|---:|
| `infra-hub` | 10 | 0 | 0 | 0 |
| `redundant-net` | 10 | 0 | 0 | 0 |

### Known Non-Blocking Issue

Both nodes still report the known failed service:

| Service | Notes |
|---|---|
| `openipmi.service` | Known existing issue; not introduced by this reboot validation |

### Result

Firewall hardening survived reboot on both nodes. Kernel updates were applied successfully. Docker services, monitoring services, DNS access, Samba access, and firewall persistence validated successfully after reboot.


---

## 2026-10-10 openipmi Service Cleanup

### Summary

Both core homelab nodes were showing a failed `openipmi.service` after reboot validation.

The service is intended for IPMI/server hardware management. The current HUB and RN systems are laptop-based nodes and do not require local IPMI driver startup.

### Action Taken

On both nodes:

| Node | Action |
|---|---|
| `infra-hub` | Disabled `openipmi.service`, masked it, and reset failed state |
| `redundant-net` | Disabled `openipmi.service`, masked it, and reset failed state |

Packages were not removed. This avoids disturbing `ipmitool` or Prometheus node exporter collector recommendations.

### Validation

Final Ansible maintenance check completed successfully:

| Node | ok | changed | unreachable | failed |
|---|---:|---:|---:|---:|
| `infra-hub` | 10 | 0 | 0 | 0 |
| `redundant-net` | 10 | 0 | 0 | 0 |

Both nodes now report zero failed systemd units.

### Result

The known `openipmi.service` failure noise has been cleared from both nodes while preserving installed monitoring-related packages.

