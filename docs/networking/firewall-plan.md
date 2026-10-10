# Firewall Plan

Planning document for LAN firewall hardening on the infra-ops homelab.

This document defines intended access rules before enabling UFW or changing host firewall policy on `infra-hub` or `redundant-net`.

No firewall changes should be applied until this plan is reviewed, validated, documented, and backed by a safe rollback path.

---

## Goals

* Keep infrastructure dashboards visible to the admin workstation.
* Keep DNS available to LAN clients.
* Prevent media, IoT, guest, and non-admin client devices from accessing infrastructure dashboards.
* Preserve HUB/RN node-to-node monitoring and service checks.
* Avoid SSH lockout.
* Avoid breaking Pi-hole, Unbound, Samba, Grafana, Prometheus, Loki, Glances, Uptime Kuma, or exporters.

---

## Current Exposure Summary

Both `infra-hub` and `redundant-net` currently expose several services broadly on the LAN.

Observed listening services include:

| Port | Service | Current Exposure | Intended Direction |
|---|---|---|---|
| 22/tcp | SSH | LAN-wide | Admin only |
| 53/tcp, 53/udp | Pi-hole DNS | LAN-wide | LAN-wide |
| 80/tcp | Pi-hole web | LAN-wide | Admin only |
| 443/tcp | Pi-hole web/HTTPS | LAN-wide | Admin only |
| 139/tcp | Samba NetBIOS | LAN-wide | Trusted devices only |
| 445/tcp | Samba SMB | LAN-wide | Trusted devices only |
| 3000/tcp | Grafana | LAN-wide | Admin only |
| 3001/tcp | Uptime Kuma | LAN-wide | Admin only |
| 3100/tcp | Loki | LAN-wide | Admin/node only |
| 61208/tcp | Glances | LAN-wide | Admin only |
| 9090/tcp | Prometheus | LAN-wide | Admin/node only |
| 9100/tcp | Node Exporter | LAN-wide | Admin/node only |
| 9617/tcp | Pi-hole Exporter | LAN-wide | Admin/node only |
| 5335/tcp, 5335/udp | Unbound | Localhost only | Keep localhost only |
| 5353/udp | Avahi/mDNS | HUB only after discovery tools | Review later |

---

## Trust Groups

### Admin

Admin devices may access infrastructure dashboards, SSH, monitoring services, and management endpoints.

* Lenovo Command Station

### Optional Trusted

Optional trusted devices may be granted limited access later if needed.

* Galaxy S25+

### Core Infrastructure

Core nodes require node-to-node monitoring and service checks.

* infra-hub
* redundant-net
* Spectrum Router

### Printer

The printer should only be reachable from devices that need to print.

* Brother Printer

### Non-Admin Clients and IoT

These devices should not access infrastructure dashboards, SSH, Prometheus, Grafana, Loki, Glances, Uptime Kuma, or Samba unless explicitly required.

* Phones
* Guest/personal devices
* Roku devices
* TVs
* Echo speaker
* PlayStation 5
* Smart frame
* Smart litter box
* Unknown/guest devices

---

## Draft Allowlist

### Allow From Any LAN Client

| Port | Purpose |
|---|---|
| 53/tcp | DNS to Pi-hole |
| 53/udp | DNS to Pi-hole |

### Allow From Lenovo Command Station

| Port | Purpose |
|---|---|
| 22/tcp | SSH administration |
| 80/tcp | Pi-hole web/admin |
| 443/tcp | Pi-hole web/admin HTTPS |
| 139/tcp | Samba browsing if needed |
| 445/tcp | Samba file access |
| 3000/tcp | Grafana |
| 3001/tcp | Uptime Kuma |
| 3100/tcp | Loki |
| 61208/tcp | Glances |
| 9090/tcp | Prometheus |
| 9100/tcp | Node Exporter |
| 9617/tcp | Pi-hole Exporter |

### Allow Between HUB and RN

HUB and RN should be allowed to communicate for monitoring, exporters, dashboards, DNS redundancy, and service checks.

Exact node-to-node rules should be reviewed before implementation.

### Deny By Default

All other inbound access should be denied by default after required allow rules are confirmed.

---

### Docker and UFW Handling

Docker-published ports require special handling because Docker creates its own iptables rules.

Observed Docker-published services include:

| Node | Port | Service |
|---|---|---|
| infra-hub | 3000/tcp | Grafana |
| infra-hub | 3001/tcp | Uptime Kuma |
| infra-hub | 3100/tcp | Loki |
| redundant-net | 3000/tcp | Grafana |
| redundant-net | 3001/tcp | Uptime Kuma |
| redundant-net | 3100/tcp | Loki |
| redundant-net | 9617/tcp | Pi-hole Exporter |

Plain UFW rules may not fully restrict these Docker-published ports.

The preferred firewall approach is:

* Use UFW for normal host services.
* Use Docker-aware filtering for Docker-published services.
* Prefer `DOCKER-USER` chain rules or container bind-address changes for Docker-exposed dashboards.
* Allow the Lenovo Command Station to reach Docker dashboards.
* Allow required HUB/RN node-to-node monitoring.
* Deny media, IoT, guest, and non-admin client access to Docker dashboards.

No Docker/UFW firewall implementation should begin until these rules are written, reviewed, and tested one node at a time.

---

## Implementation Safety Checklist

Before enabling firewall rules:

* Confirm Lenovo Command Station IP.
* Confirm HUB and RN IPs.
* Confirm SSH works to both nodes.
* Confirm current UFW status.
* Confirm Docker-published ports and Docker/UFW behavior.
* Apply rules to one node at a time.
* Keep an active SSH session open while testing.
* Add SSH allow rule before enabling UFW.
* Confirm DNS still works after rules are applied.
* Confirm dashboards remain reachable from Lenovo.
* Confirm IoT/media devices cannot reach admin dashboards.
* Document exact rules after validation.
* Commit and push documentation after changes.

---

## Deferred Items

* DHCP migration away from Spectrum router reservations.
* RN Grafana Pi-hole/NAS panel cleanup.
* Mirroring lightweight discovery tools to RN.
* Planned kernel reboot after uptime/status screenshots.
* Password rotation for any credentials exposed during troubleshooting.

---

## Status

`redundant-net` firewall hardening has been implemented and validated. `infra-hub` remains pending.

---

## redundant-net Firewall Implementation Record

`redundant-net` firewall hardening was implemented and validated on 2026-10-09 / 2026-10-10.

### Management Change

During persistence setup, installing `iptables-persistent` removed `ufw` on `redundant-net`.

`redundant-net` is currently managed directly with:

* `iptables`
* `ip6tables`
* `netfilter-persistent`

Rules were saved with `sudo netfilter-persistent save`.

Persistent rule files:

| File | Purpose |
|---|---|
| `/etc/iptables/rules.v4` | Persistent IPv4 rules |
| `/etc/iptables/rules.v6` | Persistent IPv6 rules |

`netfilter-persistent` is enabled.

### IPv4 Host Firewall

The RN IPv4 INPUT chain allows required traffic first, then drops other inbound traffic.

Current flow:

1. Allow localhost.
2. Allow established and related traffic.
3. Allow SSH from Lenovo.
4. Allow LAN DNS to Pi-hole.
5. Allow Lenovo access to required host services.
6. Allow HUB access to required RN host services.
7. Allow DHCP replies from the current router.
8. Drop all other inbound IPv4 traffic.

Implemented allow rules:

| Source | Ports | Purpose |
|---|---|---|
| `lo` | all | Localhost |
| established/related | all | Existing connections |
| `192.168.1.250` | `22/tcp` | Lenovo SSH admin |
| `192.168.1.0/24` | `53/udp`, `53/tcp` | LAN DNS to RN Pi-hole |
| `192.168.1.250` | `80,443,139,445,61208,9090,9100/tcp` | Lenovo host-service access |
| `192.168.1.225` | `22,53,80,443,445,61208,9090,9100/tcp` | HUB-to-RN checks |
| `192.168.1.1` | UDP source port `67` to destination port `68` | DHCP reply safety |
| all other IPv4 inbound | all | Drop |

### Docker Firewall

RN Docker-published ports are controlled through the `DOCKER-USER` chain.

Allowed sources:

| Source | Ports | Purpose |
|---|---|---|
| `192.168.1.250` | `3000,3001,3100,9617/tcp` | Lenovo admin to RN Docker services |
| `192.168.1.225` | `3000,3001,3100,9617/tcp` | HUB to RN Docker services |

Denied sources:

| Source | Ports | Purpose |
|---|---|---|
| `192.168.1.0/24` after earlier allow matches | `3000,3001,3100,9617/tcp` | Block non-admin LAN devices |

### IPv6 Docker Firewall

RN IPv6 access to Docker dashboard/exporter ports is blocked on `enp5s0`.

| Interface | Ports | Purpose |
|---|---|---|
| `enp5s0` | `3000,3001,3100,9617/tcp` | Block IPv6 access to Docker dashboards/exporters |

IPv6 allowlists can be added later if needed. The current management path is IPv4.

### Validation

Validated from Lenovo WSL:

| Test | Result |
|---|---|
| SSH to RN | PASS |
| DNS query to RN Pi-hole | PASS |
| Pi-hole admin web | PASS |
| Samba SMB `445/tcp` | PASS |
| Glances `61208/tcp` | PASS |
| Prometheus `9090/tcp` | PASS |
| Node Exporter `9100/tcp` | PASS |
| Grafana `3000/tcp` | PASS |
| Uptime Kuma `3001/tcp` | PASS |
| Loki `3100/tcp` | PASS |
| Pi-hole Exporter `9617/tcp` | PASS |

Validated from HUB:

| Test | Result |
|---|---|
| RN Docker ports `3000,3001,3100,9617/tcp` | PASS |

`DOCKER-USER` packet counters confirmed traffic hit the Lenovo and HUB allow rules.

### Checkpoint

Final production checkpoint saved on RN at:

`/home/ted_phipps/firewall-baselines/rn-firewall-final-checkpoint.txt`

---

## Updated Status

`redundant-net` firewall hardening is implemented, validated, checkpointed, and persisted.

`infra-hub` firewall hardening remains pending.

