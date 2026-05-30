# Device Inventory

Centralized inventory of infrastructure, client, and IoT devices within the infra-ops homelab environment.

This document serves as the authoritative reference for device roles, infrastructure services, and future inventory expansion.

---

## Infrastructure Devices

### Spectrum Router

* DHCP Server
* Default Gateway

### infra-hub

* Primary Infrastructure Node
* Pi-hole
* Unbound
* Samba
* Glances
* 1 TB NAS Storage

### redundant-net

* Secondary Infrastructure Node
* Pi-hole
* Unbound
* Samba
* Glances
* 3 TB NAS Storage

### Lenovo Command Station

* Primary Administration Workstation
* Infrastructure Management System

---

## Client Devices

### Galaxy S25+

* Primary Mobile Device

### Moto Z4

* Smart Frame Management Device
* SMB/NAS Access Testing

### Smart Frame

* LeaMea Frameo Smart Frame
* Photo Display Device
* Connected via Frameo Application

---

## IoT Devices

### Smart Litter Box

* Network Connected IoT Device

### Bedroom Roku

* Streaming Device

### Living Room TV

* Hisense Roku TV (65")

### Brother Printer

* Network Printer

### TCL Roku TV 32

* Pending Removal from Environment

---

## Future Inventory Fields

Future revisions may include:

* Hostname
* DHCP Reservation Status
* MAC Address
* Operating System
* Device Role
* Network Location
* Notes
* Service Dependencies

---

## Inventory Maintenance Notes

This inventory is intended to evolve alongside the infrastructure environment and should be updated whenever new devices, services, or infrastructure components are added, removed, or repurposed.

Detailed networking information, DHCP reservations, MAC addresses, and service dependency mappings may be documented in future revisions as the environment continues to mature.
