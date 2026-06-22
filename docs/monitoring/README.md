# Monitoring Documentation

This directory contains monitoring-related documentation, dashboards, screenshots, and operational references for the infra-ops homelab environment.

## Overview

The monitoring stack provides centralized visibility into infrastructure availability, DNS services, system health, storage utilization, container activity, and service status across both homelab hosts.

The monitoring ecosystem currently consists of Grafana, Prometheus, Loki, Promtail, Node Exporter, Pi-hole Exporter, Uptime Kuma, and Glances.

---

## Monitoring Platforms

### Grafana

Grafana serves as the primary monitoring and visualization platform for the homelab environment.

Dashboard coverage includes:

* System temperature monitoring
* CPU utilization
* Memory utilization
* Root disk utilization
* System uptime
* System load monitoring
* Pi-hole DNS activity
* Docker container logs
* Network traffic monitoring
* NAS utilization monitoring
* NAS service status monitoring
* NAS drive health monitoring
* NAS temperature monitoring

### Prometheus

Prometheus provides centralized metric collection and storage for infrastructure monitoring.

Metric sources include:

* Node Exporter
* Pi-hole Exporter
* Prometheus self-monitoring
* System performance metrics
* Storage metrics
* Network metrics

### Loki & Promtail

Loki and Promtail provide centralized log aggregation and dashboard log visualization through Grafana.

Current log monitoring includes:

* Docker container logs
* Service activity logs
* Infrastructure troubleshooting visibility

### Uptime Kuma

Uptime Kuma provides infrastructure availability monitoring and public-facing status visibility.

### Glances

Glances provides host-level system monitoring and operational visibility on both infrastructure nodes.

---

## Monitored Hosts

### infra-hub (HUB)

Services monitored:

* Pi-hole
* Unbound
* Samba NAS
* Grafana
* Prometheus
* Loki
* Promtail
* Node Exporter
* Pi-hole Exporter
* Glances
* Uptime Kuma

### redundant-net (RN)

Services monitored:

* Pi-hole
* Unbound
* Samba NAS
* Grafana
* Prometheus
* Loki
* Promtail
* Node Exporter
* Pi-hole Exporter
* Glances

---

## Uptime Kuma

Uptime Kuma is deployed on infra-hub and serves as the centralized availability monitoring platform for the homelab.

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

* HUB Samba NAS
* RN Samba NAS

### Status Page

Status Page Name:

* Infra-Ops Infrastructure Status

The status page provides a consolidated view of infrastructure availability and service health.

---

## Grafana Dashboards

### Infra-Hub Dashboard

The Infra-Hub Dashboard provides monitoring coverage for:

* System health
* Pi-hole activity
* Docker logs
* Network traffic
* NAS utilization
* NAS service status
* NAS drive health
* NAS temperature

### Redundant-Net Dashboard

The Redundant-Net Dashboard provides monitoring coverage for:

* System health
* Pi-hole activity
* Docker logs
* Network traffic
* Backup NAS utilization
* Backup NAS service status
* Backup NAS drive health
* Backup NAS temperature

---

## Screenshots

Monitoring screenshots and dashboard captures are stored in:

`screenshots/`

Current screenshot inventory includes:

* uptime-kuma-status-page.png
* hub-pihole-dashboard.png
* rn-pihole-dashboard.png
* hub-glances-dashboard.png
* rn-glances-dashboard.png
* infra-hub-grafana-dashboard.png
* rn-grafana-dashboard.png

---

## Notes

Monitoring documentation is updated as infrastructure services evolve and additional monitoring capabilities are deployed.

Future enhancements may include:

* Alerting integrations
* Notification workflows
* Historical reporting
* Additional infrastructure monitoring targets
* Expanded log aggregation and analysis
