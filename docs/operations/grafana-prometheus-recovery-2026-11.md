# Docker-to-Host Monitoring Connectivity Recovery

**Period:** Early November 2026, including November 8
**Systems:** Primary and secondary infrastructure nodes
**Technologies:** Linux, Docker, Grafana, Prometheus, Uptime Kuma, Pi-hole, Glances, Samba, iptables, netfilter-persistent

## Overview

Resolved monitoring connectivity problems caused by host firewall filtering of traffic originating from Docker containers.

Two related recovery efforts were completed:

1. Restored Grafana-to-Prometheus connectivity on both infrastructure nodes.
2. Restored Uptime Kuma monitoring connectivity on the primary node, including ICMP monitoring of the secondary node.

The underlying lesson was the same: services running successfully on Linux hosts may still be unreachable from monitoring applications running inside Docker containers.

Both recoveries preserved the restrictive firewall architecture through targeted exceptions.

---

## Incident 1 — Grafana-to-Prometheus Connectivity

### Symptoms

Grafana dashboards on both infrastructure nodes failed to retrieve expected Prometheus metrics.

Prometheus ran as a host-level service, while Grafana operated inside Docker containers. Host firewall filtering prevented the containers from reaching the required Prometheus endpoints.

### Investigation

Troubleshooting compared connectivity from the Linux hosts with connectivity from the affected container networking environments.

The investigation identified missing firewall allowances for Docker-to-host Prometheus traffic.

### Resolution

The recovery involved:

1. Identifying the relevant Docker networks and Prometheus endpoints.
2. Inspecting IPv4 firewall rules and their order.
3. Applying narrowly scoped TCP exceptions for the required Prometheus connections.
4. Preserving existing firewall protections.
5. Saving and validating the persistent firewall configuration on both nodes.

### Validation

Recovery checks confirmed:

- Successful HTTP 200 responses from the required Prometheus endpoints.
- Restored Grafana dashboard metric data.
- Successful recovery on both infrastructure nodes.
- Persistent firewall configurations saved and validated.

**Result:** Grafana-to-Prometheus connectivity restored on both nodes.

---

## Incident 2 — Uptime Kuma Monitoring Recovery

**Recovery date:** November 8, 2026
**Affected instance:** Primary-node Uptime Kuma

### Symptoms

Uptime Kuma was running in a healthy Docker container but reported multiple monitored services as unavailable.

Failures included:

- Primary-node ICMP monitoring.
- Secondary-node ICMP monitoring.
- Primary-node Pi-hole HTTP monitoring.
- Primary-node Glances HTTP monitoring.
- Primary-node Samba TCP monitoring.
- Primary-node Uptime Kuma HTTP monitoring.

The monitoring failures did not establish that all underlying services were down.

### Investigation

Connectivity tests established that:

- Uptime Kuma's internal web service responded normally.
- The primary Linux host could access Kuma through its published service endpoint.
- Kuma's Docker container could not reach that same endpoint through the host network.
- Existing firewall permissions covered another Docker network but did not cover Kuma's required Docker-to-host communication.
- The secondary node's firewall did not permit the required ICMP echo requests from the primary node.

The investigation confirmed the affected connectivity paths through before-and-after testing.

### Resolution

On the primary node:

1. Identified Kuma's Docker bridge network and container address.
2. Added a temporary Docker-to-host firewall exception.
3. Confirmed restoration of connectivity.
4. Replaced the temporary broad allowance with targeted permissions for the monitored services.
5. Included ICMP echo-request permission for primary-node monitoring.
6. Backed up and saved the firewall configuration.

The targeted TCP permissions covered the required Pi-hole, Samba, Glances, and Uptime Kuma service ports.

On the secondary node:

1. Added an ICMP echo-request exception limited to the primary node.
2. Verified successful ping responses.
3. Backed up and saved the firewall configuration.

### Validation

Final tests confirmed:

| Check | Result |
|---|---|
| Kuma container to primary-node ICMP | Successful; 0% packet loss |
| Kuma container to secondary-node ICMP | Successful; 0% packet loss |
| Kuma container to primary-node web endpoint | HTTP 302; service responding |
| Kuma container health | Healthy |
| Recent Kuma logs | No messages during the final five-minute check |
| Primary-node saved firewall exceptions | Verified |
| Secondary-node saved ICMP exception | Verified |

The earlier monitoring failures also stopped appearing in subsequent short log checks.

**Validation limitations:** The exact Uptime Kuma dashboard UP/DOWN count was not confirmed because dashboard authentication was unavailable. Saved firewall rules were inspected, but restoration after an actual reboot was not tested.

**Result:** The identified monitoring connectivity failures were corrected, with successful final connectivity tests.

---

## Firewall Change Validation Checklist

Use this checklist when introducing or changing firewall rules on either infrastructure node.

- [ ] Identify which monitoring services run directly on Linux and which run inside Docker.
- [ ] Record the relevant Docker networks, source addresses, destinations, protocols, and ports.
- [ ] Check rule ordering, especially exceptions preceding final DROP rules.
- [ ] Validate connectivity from within affected containers, not only from their Linux hosts.
- [ ] Verify Grafana-to-Prometheus connectivity and dashboard metrics.
- [ ] Verify Pi-hole and DNS monitoring from the appropriate monitoring containers.
- [ ] Verify Glances and Samba monitoring where configured.
- [ ] Verify Uptime Kuma's monitored HTTP, TCP, and ICMP endpoints on both nodes.
- [ ] Confirm the monitoring applications themselves are healthy.
- [ ] Apply least-privilege firewall exceptions instead of broadly disabling protections.
- [ ] Back up existing firewall configurations before persisting changes.
- [ ] Save rules using the established persistent firewall mechanism.
- [ ] Confirm intended rules exist in the saved configuration.
- [ ] Verify the expected monitoring behavior after changes.
- [ ] Perform post-reboot validation whenever a reboot is subsequently carried out.
- [ ] Recheck source addresses and firewall rules if Docker networks or containers are recreated.

---

## Technical Lessons

### Container Health Is Not Monitoring Health

A running or healthy Docker container does not prove that its monitoring checks can reach their destinations.

### Host Connectivity Does Not Prove Container Connectivity

Always test traffic from the same networking context as the affected application.

### Account for Every Monitoring Path

Firewall planning must include container-to-host traffic, node-to-node monitoring, ICMP, and relevant service ports.

### Apply Least-Privilege Rules

Restrict exceptions by interface, source, destination, protocol, and required service wherever practical.

### Validate and Persist Changes

Successful temporary corrections should be tested, narrowed, backed up, saved, and verified.

Reboot persistence should not be considered tested until restoration has actually been checked after a reboot.

### Review Changes Across Both Nodes

Shared monitoring architectures can produce similar failures, but each node requires independent validation.

---

## Related Documentation

- [Monitoring Architecture](../monitoring/README.md)
- [Firewall Architecture](../networking/firewall-plan.md)
- [Infrastructure Maintenance Log](maintenance-log.md)
- [Earlier Pi-hole Exporter Recovery](rn-exporter-recovery-2026-10-31.md)

The earlier Pi-hole Exporter recovery involved another Docker-to-host application API connection and remains documented separately.
