# Device Inventory

Centralized inventory of infrastructure, client, media, and IoT devices within the infra-ops homelab environment.

This document supports network inventory, firewall planning, DHCP planning, monitoring scope, and production-to-GitHub documentation sync.

Sensitive identifiers such as serial numbers, device IDs, and full MAC addresses are intentionally omitted from this document.

---

## Inventory Principles

* Treat the live homelab as the source of discovery during active build work.
* Sync validated production findings back into GitHub after discovery.
* Do not trust consumer router labels as authoritative.
* Prefer direct validation using hostnames, Pi-hole data, ARP/neighbor tables, and lightweight fingerprinting.
* Classify devices by trust group before firewall hardening.
* Do not grant dashboard, SSH, Samba, or admin access to media/IoT devices unless explicitly required.

---

## Discovery Methods Used

The current inventory was built using lightweight, read-only discovery methods:

* SSH hostname validation for infrastructure nodes
* Linux neighbor table checks with `ip neigh`
* Pi-hole FTL database read-only queries
* ARP discovery with `arp-scan`
* Targeted TCP port checks for device fingerprinting
* Roku device identification through local device-info responses
* Focused `nmap` service/OS checks for unknown devices
* Physical power testing for quiet IoT devices

Preferred ARP discovery command on HUB:

```bash
sudo arp-scan --localnet --interface=eno1 \
  --ouifile=/usr/share/arp-scan/ieee-oui.txt \
  --macfile=/etc/arp-scan/mac-vendor.txt
```

---

## Core Infrastructure

| Device | Role | Trust Group | Notes |
|---|---|---|---|
| Spectrum Router | Default gateway / DHCP server | Core infrastructure | Router app labels have proven unreliable. DHCP migration is a future roadmap item. |
| infra-hub | Primary infrastructure node | Core infrastructure | Hosts core DNS, storage, monitoring, logging, and management services. |
| redundant-net | Secondary infrastructure node | Core infrastructure | Mirrors key infrastructure services for redundancy. Hardware/vendor fingerprint may appear as Pegatron. |
| Lenovo Command Station | Primary administration workstation | Admin | Main WSL control host for Ansible, GitHub, documentation, and infrastructure management. |

---

## Admin and Trusted Devices

| Device | Trust Group | Notes |
|---|---|---|
| Lenovo Command Station | Admin | Primary trusted workstation. Should retain access to dashboards, SSH, Git workflows, and management interfaces. |
| Galaxy S25+ | Optional trusted/admin | Primary mobile device. May receive limited trusted access later if needed. Not required for initial firewall hardening. |

---

## Client Devices

| Device | Trust Group | Notes |
|---|---|---|
| Kayla's iPhone | Client / non-admin | Known client device. No infrastructure dashboard access by default. |
| Jordan's phone | Client / non-admin | Known client device. No infrastructure dashboard access by default. |
| Moto Z4 | Client / non-admin | Used for smart frame management and occasional SMB/NAS access testing. Powered on/off as needed. |
| TJ's iPhone | Client / non-admin | Known roommate/client device. No infrastructure dashboard access by default. |
| Unknown Apple-like device | Client / guest candidate | Identified through Apple/iCloud/Pinterest DNS behavior. Keep non-admin until positively identified. |

---

## Printer

| Device | Trust Group | Notes |
|---|---|---|
| Brother Printer | Printer / IoT | Confirmed reachable by direct ping when powered on. Allow printing only from needed trusted devices. No dashboard, SSH, or Samba access required. |

---

## Media and IoT Devices

| Device | Trust Group | Notes |
|---|---|---|
| PlayStation 5 | Gaming / media / non-admin | Confirmed through live ARP scan when powered on. |
| 50 inch Insignia Roku TV | Media / IoT / non-admin | Identified through Roku fingerprinting. No infrastructure access required. |
| T.K.'s Little TV Roku | Media / IoT / non-admin | Roku Express device. Bedroom Roku device. No infrastructure access required. |
| 65 inch Hisense Roku TV | Media / IoT / non-admin | Living room Roku TV. No infrastructure access required. |
| TJ's Echo speaker | Media / IoT / non-admin | Likely identified by Amazon MAC vendor and audio/media behavior. Confirm later with power test if needed. |
| Smart Frame | IoT / non-admin | Confirmed by physical power test. No infrastructure access required. |
| Smart Litter Box | IoT / non-admin | Known network-connected IoT device. Exact network identity still pending if needed. |

---

## Current Firewall Trust Model

### Admin

Admin devices may need access to infrastructure dashboards, SSH, monitoring services, and management endpoints.

* Lenovo Command Station

### Optional Trusted

Optional trusted devices may receive limited access later, but should not be included in the first firewall pass unless needed.

* Galaxy S25+

### Core Infrastructure

Core infrastructure devices require node-to-node service communication and monitoring access.

* Spectrum Router
* infra-hub
* redundant-net

### Non-Admin Clients

Client devices should not access infrastructure dashboards, SSH, Prometheus, Grafana, Loki, Glances, Uptime Kuma, or Samba unless explicitly required.

* Phones
* Guest/personal devices
* Gaming devices
* Media devices
* IoT devices

### Printer

The printer should only be reachable from devices that need to print.

---

## Known Findings

* Spectrum router device labels should not be treated as authoritative.
* Router labels previously confused infrastructure devices with printer or vendor-based names.
* Pi-hole is useful for identifying DNS clients, but not all devices use Pi-hole.
* Roku devices may bypass Pi-hole or use hard-coded DNS behavior.
* ARP visibility is more reliable than whether a screen appears off.
* Many IoT/media devices remain network-visible while the display appears off.
* The smart frame was confirmed by power testing.
* Roku devices were identified through local device fingerprinting.
* The Lenovo Command Station should be the initial admin allowlist device for firewall planning.

---

## Pending Follow-Up

* Confirm TJ's Echo speaker with a physical power test if needed.
* Identify the smart litter box network identity if required for firewall rules.
* Decide whether Galaxy S25+ should receive limited trusted/admin access.
* Plan DHCP migration away from reliance on Spectrum router reservations.
* Build firewall allowlists from trust groups before enabling UFW.
* Update RN Grafana dashboard panels for RN Pi-hole and RN NAS metrics.
* Mirror lightweight discovery tools to redundant-net after the workflow is documented and validated.
* Capture uptime/status screenshots before planned reboot or kernel maintenance.

---

## Inventory Maintenance Notes

Update this document whenever devices are added, removed, renamed, repurposed, or moved into a different trust group.

Detailed serial numbers, device IDs, and full MAC addresses should remain out of public documentation. If exact identifiers are needed, store them only in a private inventory location.
