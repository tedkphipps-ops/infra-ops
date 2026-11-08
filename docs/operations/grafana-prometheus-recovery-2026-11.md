# Grafana-to-Prometheus Connectivity Recovery

**Period:** Early November 2026

**Systems:** Primary and secondary infrastructure nodes

**Technologies:** Linux, Docker, Grafana, Prometheus, iptables, netfilter-persistent

## Overview

Resolved a monitoring connectivity problem affecting Grafana on both infrastructure nodes.

Grafana ran in Docker containers while Prometheus operated as a host-level monitoring service. Host firewall filtering prevented the containerized Grafana instances from reaching their respective Prometheus endpoints.

The issue was resolved by identifying the affected Docker-to-host traffic path and applying narrowly scoped IPv4 firewall exceptions.

Prometheus connectivity and Grafana dashboard data were restored on both nodes.

---

## Symptoms

The monitoring environment experienced connectivity failures between Grafana and Prometheus.

Observed behavior included:

- Grafana dashboards failing to retrieve expected Prometheus metrics.

- Connectivity failures originating from the containerized Grafana environment.

- Similar monitoring problems on both infrastructure nodes.

The incident affected observability rather than indicating that all underlying infrastructure services had failed.

---

## Investigation

Troubleshooting focused on the network path between Docker containers and services running directly on the Linux hosts.

The investigation identified a distinction between:

- Communication originating from the Linux host.

- Communication originating from a Docker container.

- Host firewall rules governing traffic entering the host network stack.

The relevant IPv4 firewall policy was blocking the container-to-host connection required by Grafana.

This demonstrated that a service can be operational on its host while remaining unreachable from an application running in a container on that same machine.

---

## Resolution

The recovery involved:

1. Identifying the Docker-to-host connection required by Grafana.

2. Reviewing the host's existing IPv4 firewall filtering.

3. Applying a targeted exception for the required Prometheus traffic.

4. Preserving the existing restrictive firewall approach rather than broadly opening management services.

5. Saving and validating the persistent firewall configuration.

6. Repeating the recovery and validation on both infrastructure nodes.

The correction restored the required communication without replacing the broader firewall architecture.

---

## Validation

Recovery checks confirmed:

- Prometheus connectivity from the affected container networking context.

- Successful HTTP 200 responses from the required Prometheus endpoints.

- Restored Grafana dashboard metric data.

- Successful recovery on both the primary and secondary infrastructure nodes.

- Firewall configuration saved and validated for persistence.

These results describe the checks performed during the incident recovery. They do not imply additional reboot testing.

**Result:** Grafana-to-Prometheus connectivity restored on both infrastructure nodes.

---

## Technical Lessons

### Docker Networking and Host Firewalls

Containerized applications do not necessarily share the same network access as processes running directly on the host.

Host firewall filtering and rule order must be considered when containers communicate with services bound to the host network.

### Least-Privilege Troubleshooting

Targeted firewall exceptions are preferable to disabling firewall protections or broadly allowing access to infrastructure management services.

### End-to-End Monitoring Validation

A running monitoring service does not guarantee that dashboards can retrieve its data.

Successful validation should include the application-to-data-source connection and confirmation that expected dashboard data is available.

### Consistent Recovery Across Redundant Nodes

When infrastructure nodes use similar monitoring architectures, a shared failure pattern can help identify configuration-level causes.

Each node still requires its own validation.

---

## Related Documentation

- [Monitoring Architecture](../monitoring/README.md)

- [Firewall Architecture](../networking/firewall-plan.md)

- [Infrastructure Maintenance Log](maintenance-log.md)

- [Earlier Pi-hole Exporter Recovery](rn-exporter-recovery-2026-10-31.md)

The earlier Pi-hole Exporter recovery involved a separate Docker-to-host application API connection and is documented independently.
