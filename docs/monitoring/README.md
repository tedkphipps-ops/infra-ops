# Monitoring Documentation

This directory contains monitoring-related documentation, dashboards, screenshots, and operational references for the infra-ops homelab environment.

## Overview

The monitoring stack provides visibility into infrastructure availability, DNS services, storage services, and system health across both homelab hosts.

### Monitoring Platform

* Uptime Kuma
* Glances

### Monitored Hosts

#### infra-hub (HUB)

Services monitored:

* Pi-hole
* Unbound
* Glances
* Samba
* Uptime Kuma

#### redundant-net (RN)

Services monitored:

* Pi-hole
* Unbound
* Glances
* Samba

## Uptime Kuma

Uptime Kuma is deployed on infra-hub and serves as the centralized monitoring platform for the homelab.

Current monitoring groups:

### Infrastructure

* infra-hub
* redundant-net
* Router

### DNS Services

* HUB Pi-hole
* RN Pi-hole

### Monitoring Services

* HUB Glances
* RN Glances

### Storage Services

* HUB Samba
* RN Samba

## Status Page

A public-facing status page is maintained through Uptime Kuma.

Status Page Name:

* Infra-Ops Infrastructure Status

The status page provides a consolidated view of infrastructure availability and service health.

## Screenshots

Monitoring screenshots and dashboard captures are stored in:

```text
screenshots/
```

Current contents:

* uptime-kuma-status-page.png
  * Infrastructure status page overview

## Notes

Monitoring configurations are updated as infrastructure services evolve.

Additional monitoring targets, alerting integrations, and service checks may be added in future development phases.
