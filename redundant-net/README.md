# Redundant Infrastructure Node

## Overview

This module documents the deployment, configuration, and operational role of the `redundant-net` infrastructure node within the `infra-ops` homelab environment.

`redundant-net` functions as the secondary infrastructure node, providing redundancy, monitoring, storage services, DNS resiliency, and operational continuity for the homelab ecosystem.

Originally deployed as a DNS failover platform, the node has evolved into a fully integrated infrastructure system mirroring many core services hosted on `infra-hub`.

---

## Infrastructure Role

`redundant-net` serves as the secondary infrastructure node within the environment.

Primary responsibilities include:

* DNS redundancy
* Recursive DNS resolution
* Infrastructure monitoring
* Secondary NAS services
* Log aggregation support
* Metrics collection
* Service continuity
* Operational resiliency

The node operates alongside `infra-hub` to provide redundancy and infrastructure resilience across critical homelab services.

---

## Infrastructure Architecture

| Node            | Role                          | Core Services                                                                                         |
| --------------- | ----------------------------- | ----------------------------------------------------------------------------------------------------- |
| `infra-hub`     | Primary Infrastructure Node   | Pi-hole, Unbound, Samba NAS, Glances, Grafana, Prometheus, Loki, Promtail, Node Exporter, Uptime Kuma |
| `redundant-net` | Secondary Infrastructure Node | Pi-hole, Unbound, Samba NAS, Glances, Grafana, Prometheus, Loki, Promtail, Node Exporter, Uptime Kuma |

---

## Services Deployed

### DNS Services

* Pi-hole
* Unbound
* Pi-hole Exporter

### Monitoring & Observability

* Grafana
* Prometheus
* Loki
* Promtail
* Node Exporter
* Glances
* Uptime Kuma

### Storage Services

* Samba NAS
* Secondary NAS Storage

---

## Secondary NAS Infrastructure

`redundant-net` provides an independent NAS storage volume, separate from the primary NAS.

Current storage configuration:

| Component  | Value                                      |
| ---------- | ------------------------------------------ |
| Mount Path | `/mnt/rn-nas`                              |
| Filesystem | NTFS3                                      |
| Capacity   | Approximately 3 TB                         |
| Purpose    | Independent Secondary Storage              |

Automated backup synchronization and disaster recovery remain future work.

---

## Monitoring Integration

The node participates in the centralized monitoring ecosystem and includes:

* System temperature monitoring
* CPU utilization monitoring
* Memory utilization monitoring
* Root filesystem monitoring
* Network traffic monitoring
* Docker container log monitoring
* NAS utilization monitoring
* NAS drive health monitoring
* Pi-hole metrics monitoring

Monitoring visibility is provided through the Redundant-Net Grafana Dashboard.

---

## DNS Redundancy Design

The environment utilizes dual-node DNS infrastructure.

### Primary DNS Node

* `infra-hub`

### Secondary DNS Node

* `redundant-net`

This design provides:

* Recursive DNS redundancy
* Improved DNS resiliency
* Reduced dependency on external providers
* Improved operational continuity

---

## Validation & Testing

The following validation procedures were performed:

* Verified Pi-hole functionality
* Verified Unbound recursive DNS operation
* Confirmed Prometheus metrics collection
* Confirmed Grafana dashboard functionality
* Verified Loki log aggregation
* Verified NAS accessibility
* Verified Node Exporter metrics
* Validated monitoring dashboards
* Tested DNS redundancy functionality
* Confirmed service startup persistence after reboot

---

## Infrastructure Integration

`redundant-net` integrates directly with the following infrastructure modules:

* `pihole-setup`
* `unbound`
* `samba-nas`
* `glances-monitoring`
* `monitoring`
* `infra-hub`

Together, these services provide a resilient and observable infrastructure platform focused on redundancy, monitoring, and operational continuity.

---

## Future Improvements

Planned future enhancements include:

* Automated configuration replication
* Enhanced backup automation
* Additional storage expansion
* Reverse proxy integration
* VLAN-aware monitoring
* Infrastructure alerting
* Expanded redundancy testing
* Multi-node service synchronization

---

## Operational Notes

The evolution of `redundant-net` transformed the environment from a single-node homelab into a resilient multi-node infrastructure platform.

The node now provides redundancy across DNS, monitoring, storage, and observability services while supporting future infrastructure growth and disaster recovery planning.

This deployment represents a significant milestone in the maturation of the `infra-ops` ecosystem.
