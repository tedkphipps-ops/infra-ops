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

Planning only. No firewall rules have been applied from this document yet.
