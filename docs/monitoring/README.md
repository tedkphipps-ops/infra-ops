# Homelab Monitoring and Observability

## Overview

The homelab uses a multi-service monitoring platform to
provide visibility into two Linux infrastructure nodes.

Monitoring covers system health, DNS, storage, containers,
logs, performance metrics, and service availability.

The architecture includes redundant availability monitoring
to improve visibility during infrastructure failures.

## Monitoring Technology

| Component        | Function                        |
| ---------------- | ------------------------------- |
| Grafana          | Dashboards and visualization    |
| Prometheus       | Metrics collection and storage  |
| Loki             | Centralized log aggregation     |
| Promtail         | System and service log shipping |
| Node Exporter    | Linux host metrics              |
| Pi-hole Exporter | DNS service metrics             |
| Uptime Kuma      | Availability monitoring         |
| Glances          | Real-time system monitoring     |

## Infrastructure Architecture

The environment uses two infrastructure nodes.

### Primary Node — `infra-hub`

Provides core infrastructure services, monitoring
dashboards, metrics collection, logging, and
availability monitoring.

### Secondary Node — `redundant-net`

Provides supporting infrastructure services,
redundant monitoring, and additional observability.

Both nodes participate in infrastructure health
and availability monitoring.

Actual server addresses and connection information
are maintained privately.

## Metrics Architecture

Prometheus collects metrics from configured exporters.

The basic metrics pipeline is:

    Linux hosts --> Node Exporter --> Prometheus
    DNS services --> Pi-hole Exporter --> Prometheus
    Prometheus --> Grafana dashboards

Monitored metrics include:

- CPU and memory utilization
- System load and uptime
- Filesystem and storage capacity
- Network activity
- DNS queries and blocking activity
- Infrastructure performance trends

## Centralized Logging

Promtail collects configured system and service logs.

The logging pipeline is:

    System logs --> Promtail --> Loki --> Grafana

Loki provides log storage and querying.

Grafana allows operators to investigate service
behavior alongside infrastructure metrics.

## Grafana Dashboards

Grafana dashboards provide visibility into:

- Host resource utilization
- Storage and NAS health
- DNS performance
- Container and service health
- Metrics collection
- System and service logs

Dashboard URLs, credentials, and operational
data-source configuration remain private.

## Redundant Availability Monitoring

Uptime Kuma is deployed on both infrastructure nodes.

Each instance monitors critical infrastructure services,
and both instances monitor each other.

Monitoring categories include:

- Infrastructure availability
- DNS services
- Monitoring services
- Storage availability

Peripheral devices are excluded when their
availability data creates unnecessary monitoring noise.

### Historical Validation

On October 31, 2026, both monitoring instances
had a verified baseline of:

| Instance              | Up  | Down |
| --------------------- | --- | ---- |
| Primary Uptime Kuma   | 13  | 0    |
| Secondary Uptime Kuma | 13  | 0    |

These values represent historical validation,
not current live monitoring status.

## Host-Level Monitoring

Glances provides real-time operational visibility.

Node Exporter provides host metrics for Prometheus.

Together, these tools support investigation of:

- CPU and memory pressure
- Disk capacity and utilization
- Storage availability
- Network activity
- Running processes
- Host uptime and resource trends

## Operational Validation

Monitoring maintenance includes:

- Verifying collection services
- Checking exporter availability
- Reviewing dashboard data
- Confirming logging pipeline functionality
- Validating storage availability
- Checking container health
- Reviewing redundant monitoring coverage

Ansible provides repeatable infrastructure
maintenance and validation checks.

## Troubleshooting and Recovery

The monitoring platform has supported troubleshooting
of resource pressure, storage availability,
and container networking problems.

A documented October 2026 recovery restored
exporter connectivity after a firewall restriction
blocked required container-to-host communication.

Validation confirmed restored metrics collection
and persistent firewall configuration.

See [RN Pi-hole Exporter Recovery](../operations/rn-exporter-recovery-2026-10-31.md)
for the documented incident, corrective action, and validation.

Sensitive connection details remain private.

## Screenshots and Evidence

Sanitized monitoring screenshots may be published
to demonstrate dashboards and validation results.

Screenshots must be reviewed for addresses,
identifiers, private URLs, and other sensitive data.

Historical screenshots do not represent live status.

## Future Improvements

Planned improvements include:

- Updated observability architecture diagrams
- Additional Ansible validation playbooks
- Improved DNS health testing
- Further monitoring redundancy
- Refinement of alerting and dashboard coverage

## Repository Security

This public document intentionally excludes
operational network addresses, private endpoints,
credentials, and identifying device information.

See docs/security/public-repository-policy.md
for repository publication requirements.
