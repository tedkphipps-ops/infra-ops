# Maintenance Log

This log records validated maintenance events for the homelab.

---

## 2026-10-10 Firewall Hardening and Kernel Reboot Validation

### Summary

Both core homelab nodes were rebooted one at a time after firewall hardening and pending kernel updates.

Nodes:

| Node | Hostname | IP |
|---|---|---|
| HUB | `infra-hub` | `192.168.1.225` |
| RN | `redundant-net` | `192.168.1.237` |

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

