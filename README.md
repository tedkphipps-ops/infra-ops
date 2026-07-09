# Infra-Ops

A centralized Linux infrastructure operations repository focused on DNS services, monitoring and observability, storage services, infrastructure resiliency, and self-hosted systems administration.

This repository documents the deployment, configuration, monitoring, troubleshooting, and ongoing evolution of a multi-node homelab environment built around operational reliability and documentation-driven infrastructure management.

---

## Overview

The `infra-ops` environment is built around a modular infrastructure philosophy emphasizing:

* Infrastructure Resiliency
* Operational Continuity
* Recursive DNS Independence
* Monitoring And Observability
* Documentation-Driven Operations
* Service Redundancy
* Centralized Storage Services
* Scalable Infrastructure Standardization

The environment leverages Ubuntu Server as the foundational platform supporting DNS services, monitoring platforms, storage services, and redundancy-focused architecture.

---

## Infrastructure Architecture

The environment currently operates across two infrastructure nodes:

| Node            | Role                          |
| --------------- | ----------------------------- |
| `infra-hub`     | Primary Infrastructure Node   |
| `redundant-net` | Secondary Infrastructure Node |

### Core Infrastructure Services

#### DNS Services

* Pi-hole
* Unbound

#### Monitoring & Observability

* Grafana
* Prometheus
* Loki
* Promtail
* Node Exporter
* Pi-hole Exporter
* Uptime Kuma
* Glances

#### Storage Services

* Samba NAS
* HUB NAS Storage
* RN Backup NAS Storage

---

## Infrastructure Modules

| Module               | Purpose                                                 |
| -------------------- | ------------------------------------------------------- |
| `pihole-setup`       | DNS Filtering Infrastructure And DNS Observability      |
| `unbound`            | Recursive DNS Infrastructure And Private DNS Resolution |
| `redundant-net`      | Secondary Infrastructure Node And Resiliency Platform   |
| `glances-monitoring` | Host-Level Monitoring And Operational Visibility        |
| `samba-nas`          | Network Attached Storage And Infrastructure Persistence |
| `system-maintenance` | Routine maintenance workflow, health checks, update validation, and post-reboot verification |

---

## Monitoring Stack

The environment utilizes a centralized monitoring and observability stack.

### Monitoring Components

| Service          | Purpose                 |
| ---------------- | ----------------------- |
| Grafana          | Dashboard Visualization |
| Prometheus       | Metrics Collection      |
| Loki             | Log Aggregation         |
| Promtail         | Log Shipping            |
| Node Exporter    | System Metrics          |
| Pi-hole Exporter | DNS Metrics             |
| Uptime Kuma      | Service Monitoring      |
| Glances          | Host-Level Monitoring   |

### Monitoring Coverage

Current monitoring visibility includes:

* CPU Utilization
* Memory Utilization
* Root Filesystem Monitoring
* NAS Utilization Monitoring
* DNS Activity Monitoring
* Network Traffic Monitoring
* Container Log Visibility
* Service Health Monitoring
* Infrastructure Availability Monitoring
* Drive Health Monitoring
* Drive Temperature Monitoring

---

## DNS Architecture

The environment utilizes a dual-node DNS architecture.

### Primary DNS Infrastructure

* Pi-hole
* Unbound
* Hosted On `infra-hub`

### Secondary DNS Infrastructure

* Pi-hole
* Unbound
* Hosted On `redundant-net`

This architecture provides:

* Recursive DNS Independence
* DNS Resiliency
* Reduced Dependency On External DNS Providers
* Improved Fault Tolerance
* Infrastructure Continuity

---

## Storage Architecture

The infrastructure currently utilizes dual NAS deployments.

| Node            | Mount Path     | Purpose             |
| --------------- | -------------- | ------------------- |
| `infra-hub`     | `/mnt/hub-nas` | Primary NAS Storage |
| `redundant-net` | `/mnt/rn-nas`  | Backup NAS Storage  |

Storage visibility is integrated into the monitoring stack through Prometheus, Node Exporter, Grafana, and Glances.

---

## Documentation Structure

### Diagrams

```text
docs/diagrams/
```

Contains:

* Network Topology Diagrams
* DNS Flow Diagrams
* Service Dependency Diagrams

### Monitoring

```text
docs/monitoring/
```

Contains:

* Monitoring Documentation
* Dashboard Screenshots
* Monitoring Architecture References

---

## Operational Practices

Operational workflows within the environment include:

* Service Validation Testing
* Recursive DNS Verification
* Infrastructure Monitoring
* DNS Redundancy Validation
* NAS Monitoring Validation
* Operational Troubleshooting Documentation
* Configuration Standardization
* Incremental Infrastructure Refinement
* System Maintenance and Post-Reboot Validation

---

## Future Roadmap

Planned future initiatives include:

* Infrastructure Alerting
* VLAN And Subnet Segmentation
* Reverse Proxy Deployment
* Additional Backup Automation
* Disaster Recovery Testing
* Infrastructure-As-Code Workflows
* Expanded Monitoring Capabilities
* Long-Term Observability Enhancements

---

## Repository Philosophy

This repository documents the progression from isolated self-hosted services into a structured infrastructure operations environment emphasizing resiliency, observability, documentation, and continuous improvement.

The goal is not only to deploy services, but to build, validate, document, monitor, and continuously improve a resilient infrastructure ecosystem.

---

## Maintainer

**Ted Phipps**

### Focus Areas

* Infrastructure Operations
* Systems Administration
* Monitoring & Observability
* DNS Services
* Linux Infrastructure
