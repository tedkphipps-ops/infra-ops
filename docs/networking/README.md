# Networking Documentation

## Overview

This module contains networking-related documentation for the infra-ops homelab environment.

The networking section serves as the central location for network architecture documentation, device inventories, DNS infrastructure references, and future network planning initiatives.

The goal of this documentation is to improve infrastructure visibility, simplify management tasks, and support long-term homelab growth.

---

## Documentation Contents

### Device Inventory

The device inventory provides a centralized reference for infrastructure, client, and IoT devices currently deployed within the environment.

Current inventory categories include:

* Infrastructure Devices
* Client Devices
* IoT Devices

Reference:

* device-inventory.md

---

## Infrastructure Overview

The networking environment currently consists of:

### Infrastructure Nodes

* infra-hub

  * Primary Infrastructure Node
  * Pi-hole
  * Unbound
  * Samba
  * Glances

* redundant-net

  * Secondary Infrastructure Node
  * Pi-hole
  * Unbound
  * Samba
  * Glances

### Network Services

Core network services currently include:

* DNS Filtering (Pi-hole)
* Recursive DNS Resolution (Unbound)
* File Sharing (Samba)
* Infrastructure Monitoring (Glances)

---

## Documentation Objectives

This section supports the following goals:

* Network visibility
* Infrastructure documentation
* Device tracking
* DNS architecture documentation
* Service dependency documentation
* Infrastructure scalability planning
* Homelab operational continuity

---

## Future Development

* Service Dependency Mapping
* DHCP Reservation Documentation

---

## Notes

Networking documentation within the infra-ops repository is intended to evolve alongside the homelab environment.

Documentation is updated as infrastructure components are added, modified, repurposed, or retired from service.

The objective is to maintain clear, scalable, and practical documentation that supports both current operations and future infrastructure growth.
