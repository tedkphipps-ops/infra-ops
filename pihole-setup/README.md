# Pi-hole Infrastructure Deployment

## Overview

This module documents the deployment, configuration, and operational integration of Pi-hole DNS filtering services within the `infra-ops` homelab environment.

Pi-hole serves as the primary DNS filtering platform across the infrastructure and provides centralized DNS visibility, advertisement blocking, local DNS management, and DNS observability.

The environment currently utilizes dual Pi-hole deployments to support DNS resiliency and service continuity.

### Infrastructure Nodes

| Node            | Role                              |
| --------------- | --------------------------------- |
| `infra-hub`     | Primary DNS Infrastructure Node   |
| `redundant-net` | Secondary DNS Infrastructure Node |

---

## Infrastructure Objectives

* Deploy centralized DNS filtering services
* Improve network visibility and DNS observability
* Reduce advertising and tracking traffic
* Support recursive DNS resolution through Unbound
* Establish DNS redundancy across infrastructure nodes
* Improve infrastructure resiliency
* Support long-term DNS monitoring and analytics

---

## Infrastructure Architecture

Pi-hole operates as the DNS filtering layer within the infrastructure stack.

Current DNS architecture:

```text
Client Devices
       │
       ▼
    Pi-hole
       │
       ▼
    Unbound
       │
       ▼
Recursive DNS Resolution
```

This architecture is deployed across both infrastructure nodes to provide DNS resiliency and redundancy.

---

## Network Configuration

| Component              | Value           |
| ---------------------- | --------------- |
| Primary DNS Node       | `infra-hub`     |
| Secondary DNS Node     | `redundant-net` |
| DNS Filtering Platform | Pi-hole         |
| Recursive Resolver     | Unbound         |
| Remote Administration  | OpenSSH         |

---

## Pi-hole Deployment

Pi-hole was deployed on Ubuntu Server infrastructure nodes using the official installation methodology.

### Core Features Enabled

* DNS query logging
* Web administration interface
* Local DNS management
* Blocklist integration
* Network-wide advertisement filtering
* DNS statistics and reporting
* Client activity monitoring
* DNS observability

---

## Recursive DNS Integration

Pi-hole integrates directly with Unbound to provide private recursive DNS resolution.

Benefits include:

* Reduced reliance on third-party DNS providers
* Increased DNS privacy
* Independent recursive DNS infrastructure
* Improved DNS query integrity
* Infrastructure-level DNS resiliency

---

## Monitoring Integration

Pi-hole is integrated into the centralized monitoring ecosystem.

Monitoring components include:

* Pi-hole Exporter
* Grafana
* Prometheus
* Uptime Kuma
* Glances

Current monitoring coverage includes:

* DNS query activity
* Query volume trends
* Advertisement blocking statistics
* Block percentage metrics
* Service availability
* DNS infrastructure health

---

## Grafana Dashboard Integration

Pi-hole metrics are visualized through Grafana dashboards hosted on both infrastructure nodes.

Current dashboard visibility includes:

* DNS Activity
* Ads Blocked Today
* Block Percentage
* Client Activity
* DNS Query Trends
* Service Health Monitoring

Monitoring screenshots are maintained within:

```text
docs/monitoring/screenshots/
```

---

## Validation & Testing

The following validation procedures were performed:

* Verified DNS resolution functionality
* Confirmed network-wide client connectivity
* Validated Pi-hole web interface access
* Tested DNS filtering across client devices
* Confirmed recursive DNS integration through Unbound
* Verified Prometheus metric collection
* Confirmed Grafana dashboard functionality
* Validated DNS redundancy across infrastructure nodes
* Verified service persistence following reboot

---

## DNS Redundancy Design

The infrastructure utilizes dual Pi-hole deployments to reduce dependency on a single DNS server.

### Primary DNS Infrastructure

* `infra-hub`

### Secondary DNS Infrastructure

* `redundant-net`

This design provides:

* DNS service continuity
* Improved fault tolerance
* Recursive DNS redundancy
* Increased infrastructure resiliency
* Reduced service interruption risk

---

## Infrastructure Integration

Pi-hole integrates with the following infrastructure modules:

* `unbound`
* `redundant-net`
* `glances-monitoring`
* `samba-nas`
* `monitoring`

Together these services provide centralized DNS management, monitoring, observability, storage services, and infrastructure resiliency.

---

## Future Improvements

Planned future enhancements include:

* Additional DNS analytics
* Infrastructure alerting integration
* Expanded DNS monitoring capabilities
* Future VLAN and subnet-aware DNS policies
* Enhanced resiliency testing
* Continued documentation standardization

---

## Operational Notes

The Pi-hole deployment marked the first major infrastructure service within the homelab and served as the foundation for the broader infrastructure ecosystem that followed.

The platform evolved from a single DNS filtering server into a dual-node DNS architecture supporting recursive DNS resolution, centralized monitoring, and infrastructure resiliency.

The integration of Unbound, Pi-hole Exporter, Grafana, Prometheus, and Uptime Kuma transformed DNS services into a fully observable infrastructure component and established the foundation for future infrastructure growth across the `infra-ops` environment.
