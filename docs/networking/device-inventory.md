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
* Use device trust groups to review and refine existing firewall rules.
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

| Device            | Role                               | Trust Group         | Notes                                                    |
| ----------------- | ---------------------------------- | ------------------- | -------------------------------------------------------- |
| ISP router        | Default gateway / DHCP server      | Core infrastructure | Gateway and DHCP; migration planned.                     |
| infra-hub         | Primary infrastructure node        | Core infrastructure | Hosts DNS, storage, monitoring, logging, and management. |
| redundant-net     | Secondary infrastructure node      | Core infrastructure | Provides redundant infrastructure services.              |
| Admin workstation | Primary administration workstation | Admin               | Ansible, Git, documentation, and administration.         |

---

## Admin and Trusted Devices

| Device                 | Trust Group      | Notes                                                            |
| ---------------------- | ---------------- | ---------------------------------------------------------------- |
| Lenovo Command Station | Admin            | Primary management host; retain SSH and dashboard access.        |
| ROG Mobile Command     | Admin            | GitHub and SSH access verified.                                  |
| Galaxy S25+            | Optional trusted | Limited access may be considered during future hardening.        |

---

## Client Devices

| Device                    | Trust Group              | Notes                                              |
| ------------------------- | ------------------------ | -------------------------------------------------- |
| Client iPhone A           | Client / non-admin       | No infrastructure dashboard access by default.     |
| Client iPhone B           | Client / non-admin       | No infrastructure dashboard access by default.     |
| Secondary Android phone   | Client / non-admin       | Occasional smart-frame management and NAS testing. |
| Client iPhone C           | Client / non-admin       | No infrastructure dashboard access by default.     |
| Unidentified Apple device | Client / guest candidate | Identification pending; keep non-admin.            |

---

## Printer

| Device          | Trust Group   | Notes                                                      |
| --------------- | ------------- | ---------------------------------------------------------- |
| Brother Printer | Printer / IoT | Ping-verified; permit printing from approved devices only. |

---

## Media and IoT Devices

| Device                   | Trust Group                | Notes                                            |
| ------------------------ | -------------------------- | ------------------------------------------------ |
| PlayStation 5            | Gaming / media / non-admin | Identified through ARP discovery.                |
| 50-inch Insignia Roku TV | Media / IoT / non-admin    | Identified through Roku fingerprinting.          |
| Roku Express             | Media / IoT / non-admin    | Roku streaming device; no infrastructure access. |
| 65-inch Hisense Roku TV  | Media / IoT / non-admin    | Roku TV; no infrastructure access.               |
| Amazon Echo speaker      | Media / IoT / non-admin    | Tentative identification; power test pending.    |
| Smart frame              | IoT / non-admin            | Confirmed through physical power testing.        |
| Smart litter box         | IoT / non-admin            | Network identity confirmation pending.           |

---

## Planned Firewall Trust Model

### Admin

Admin devices may need access to infrastructure dashboards, SSH, monitoring services, and management endpoints.

* Lenovo Command Station
* ROG Mobile Command (GitHub and SSH verified)

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
* Lenovo Command Station is the initial admin allowlist device; ROG Mobile Command has verified GitHub and SSH access.

---

## Pending Follow-Up

* Confirm Echo smart speaker with a physical power test if needed.
* Identify the smart litter box network identity if required for firewall rules.
* Decide whether Galaxy S25+ should receive limited trusted/admin access.
* Plan DHCP migration away from reliance on Spectrum router reservations.
* Review existing `iptables`/`ip6tables` allowlists against trust groups; validate changes before persisting them.
* Update RN Grafana dashboard panels for RN Pi-hole and RN NAS metrics.
* Mirror lightweight discovery tools to redundant-net after the workflow is documented and validated.
* Capture uptime/status screenshots before planned reboot or kernel maintenance.

---

## Inventory Maintenance Notes

Update this document whenever devices are added, removed, renamed, repurposed, or moved into a different trust group.

Detailed serial numbers, device IDs, and full MAC addresses should remain out of public documentation. If exact identifiers are needed, store them only in a private inventory location.
