# Monitoring Documentation

## Overview

This directory documents the monitoring and observability stack used in the `infra-ops` homelab environment.

The monitoring ecosystem provides visibility into host health, DNS activity, service availability, logs, metrics, storage usage, and infrastructure uptime across both primary and secondary infrastructure nodes.

The environment currently includes monitoring coverage for:

* `infra-hub`
* `redundant-net`
* DNS services
* NAS storage
* Docker containers
* Host-level system health
* Service availability
* Logs and metrics
* Uptime and availability checks

---

## Monitoring Stack

The monitoring ecosystem currently consists of:

* Grafana
* Prometheus
* Loki
* Promtail
* Node Exporter
* Pi-hole Exporter
* Uptime Kuma
* Glances

---

## Monitoring Roles

| Service | Role |
| --- | --- |
| Grafana | Dashboard visualization |
| Prometheus | Metrics collection |
| Loki | Log aggregation |
| Promtail | Log shipping |
| Node Exporter | Host metrics exporter |
| Pi-hole Exporter | Pi-hole metrics exporter |
| Uptime Kuma | Service availability monitoring |
| Glances | Host-level monitoring dashboard |

---

## Infrastructure Nodes

### infra-hub

`infra-hub` is the primary infrastructure node.

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

### redundant-net

`redundant-net` is the secondary infrastructure node.

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

---

## Grafana

Grafana provides dashboard visibility for infrastructure monitoring data.

Grafana is used to visualize:

* CPU utilization
* Memory utilization
* Disk usage
* NAS storage usage
* Pi-hole DNS metrics
* Prometheus metrics
* Loki log data
* Service and host health

Grafana dashboards are available on both infrastructure nodes.

| Node | URL | Role |
| --- | --- | --- |
| `infra-hub` | `http://REDACTED_PRIVATE_IP:3000` | Primary Grafana dashboard |
| `redundant-net` | `http://REDACTED_PRIVATE_IP:3000` | Secondary Grafana dashboard |

---

## Prometheus

Prometheus collects infrastructure metrics from exporters and monitored services.

Prometheus is used to collect metrics from:

* Node Exporter
* Pi-hole Exporter
* Prometheus itself
* Loki-related monitoring targets where applicable

Prometheus supports Grafana dashboards by providing metrics for system health, DNS visibility, storage utilization, and infrastructure trend analysis.

---

## Loki

Loki provides log aggregation for the monitoring stack.

Loki stores and serves log data that can be queried and visualized through Grafana.

In this environment:

* Loki acts as the log backend.
* Promtail ships logs into Loki.
* Grafana displays log data from Loki.

Loki is monitored as part of the infrastructure monitoring stack because log visibility is important for troubleshooting service failures and system behavior.

---

## Promtail

Promtail collects and ships logs to Loki.

Promtail supports log visibility by forwarding system and service logs into the logging pipeline.

The basic log flow is:

```text
System / Service Logs -> Promtail -> Loki -> Grafana
```

---

## Node Exporter

Node Exporter provides host-level metrics to Prometheus.

Metrics include:

* CPU usage
* Memory usage
* Disk usage
* Filesystem data
* Network statistics
* System load
* Host uptime

Node Exporter is part of the metrics collection layer for both `infra-hub` and `redundant-net`.

---

## Pi-hole Exporter

Pi-hole Exporter exposes Pi-hole metrics for Prometheus and Grafana.

It supports DNS visibility by exporting data such as:

* Query counts
* Blocked queries
* Allowed queries
* DNS activity trends
* Pi-hole service metrics

Pi-hole Exporter allows Pi-hole activity to be included in Grafana dashboards.

---

## Glances

Glances provides host-level monitoring through a web dashboard.

Glances is useful for quick operational review of:

* CPU usage
* Memory usage
* Disk usage
* Network activity
* Processes
* System load
* Temperatures
* Host status

Glances dashboards are available on both infrastructure nodes.

| Node | URL | Role |
| --- | --- | --- |
| `infra-hub` | `http://REDACTED_PRIVATE_IP:61208` | Primary host monitoring dashboard |
| `redundant-net` | `http://REDACTED_PRIVATE_IP:61208` | Secondary host monitoring dashboard |

---

## Uptime Kuma

Uptime Kuma is deployed on both infrastructure nodes to provide redundant availability monitoring for the homelab environment.

The primary Uptime Kuma dashboard runs on `infra-hub`.

The secondary Uptime Kuma dashboard runs on `redundant-net`.

Each dashboard monitors core infrastructure services, and both dashboards monitor each other to improve troubleshooting visibility if one infrastructure node becomes unavailable.

| Node | URL | Role |
| --- | --- | --- |
| `infra-hub` | `http://REDACTED_PRIVATE_IP:3001` | Primary availability monitoring dashboard |
| `redundant-net` | `http://REDACTED_PRIVATE_IP:3001` | Secondary availability monitoring dashboard |

### Current Uptime Kuma Baseline

Current expected Uptime Kuma baseline:

| Dashboard | Expected Status |
| --- | --- |
| HUB Kuma | 13 up, 0 down |
| RN Kuma | 13 up, 0 down |

The Brother printer and Litter-Robot monitors are intentionally excluded from both dashboards for now because they are not reliable infrastructure targets at this stage.

### HUB Kuma

HUB Kuma monitors core infrastructure services across the homelab environment.

Current HUB Kuma coverage includes:

* `infra-hub`
* `redundant-net`
* `spectrum-router`
* HUB Pi-hole
* RN Pi-hole
* HUB Glances
* RN Glances
* HUB Grafana
* HUB Loki
* HUB Uptime Kuma
* RN Uptime Kuma
* HUB Samba NAS
* RN Samba NAS

### RN Kuma

RN Kuma provides redundant monitoring from the secondary infrastructure node.

Current RN Kuma coverage includes:

* `infra-hub`
* `redundant-net`
* `spectrum-router`
* HUB Pi-hole
* RN Pi-hole
* HUB Glances
* RN Glances
* HUB Grafana
* HUB Loki
* HUB Uptime Kuma
* RN Uptime Kuma
* HUB Samba NAS
* RN Samba NAS

### Uptime Kuma Monitor Categories

#### Infrastructure

* `infra-hub`
* `redundant-net`
* `spectrum-router`

#### DNS Services

* HUB Pi-hole
* RN Pi-hole

#### Monitoring Services

* HUB Glances
* RN Glances
* HUB Grafana
* HUB Loki
* HUB Uptime Kuma
* RN Uptime Kuma

#### Storage Services

* HUB Samba NAS
* RN Samba NAS

---

## Monitoring Tags

Uptime Kuma monitors use tags to group services by role.

Current tag categories include:

* Infrastructure
* DNS
* Monitoring
* Storage

Peripheral monitoring is intentionally excluded from the current baseline until the related devices are stable enough to provide useful availability data.

---

## Monitoring Philosophy

The monitoring stack is designed to provide practical operational visibility rather than noisy or unnecessary alerts.

Current monitoring priorities:

* Core infrastructure availability
* DNS service availability
* Monitoring platform health
* NAS and Samba reachability
* Host-level health visibility
* Log and metrics availability
* Redundant dashboard access

Monitoring should help answer operational questions such as:

* Is the node online?
* Is DNS reachable?
* Is the dashboard reachable?
* Is the NAS reachable?
* Are logs available?
* Are metrics available?
* Can one node still monitor the other if a failure occurs?

---

## Current Production Baseline

Current monitoring production baseline:

* `infra-hub` hosts Grafana, Loki, Promtail, Pi-hole Exporter, and Uptime Kuma containers.
* `redundant-net` hosts Grafana, Loki, Promtail, Pi-hole Exporter, and Uptime Kuma containers.
* HUB Kuma and RN Kuma both monitor core infrastructure services.
* HUB Kuma and RN Kuma monitor each other.
* HUB Kuma currently reports 13 up and 0 down.
* RN Kuma currently reports 13 up and 0 down.
* Printer and Litter-Robot monitors are intentionally excluded for now.
* `/mnt/hub-nas` is monitored through HUB Samba NAS reachability.
* `/mnt/rn-nas` is monitored through RN Samba NAS reachability.
* Ansible maintenance validation confirms Docker service status and running containers on both nodes.

---

## Screenshots

Monitoring screenshots are stored in:

```text
docs/monitoring/screenshots/
```

Screenshots are used to document dashboard state and monitoring visibility.

Because monitoring values change over time, screenshots should be treated as examples of dashboard configuration and visibility rather than fixed production metrics.

---

## Related Documentation

Related repository documentation:

* `ansible/README.md`
* `system-maintenance/README.md`
* `glances-monitoring/README.md`
* `pihole-setup/README.md`
* `redundant-net/README.md`
* `samba-nas/README.md`
* `docs/diagrams/README.md`

---

## Future Improvements

Planned or potential monitoring improvements:

* Update the service dependency diagram to reflect the full observability stack.
* Add additional Ansible validation playbooks for DNS, storage, and post-reboot checks.
* Review whether Unbound should be monitored directly through command-based DNS validation.
* Add alerting workflows after monitoring baselines are stable.
* Revisit printer and Litter-Robot monitoring when those devices become reliable monitoring targets.
* Continue refining dashboard documentation as production monitoring changes.
