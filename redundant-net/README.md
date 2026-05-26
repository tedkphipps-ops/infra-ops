# Redundant Infrastructure DNS Architecture

## Overview

This module documents the deployment and operational design of the secondary infrastructure redundancy node within the `infra-ops` environment.

The `redundant-net` node provides infrastructure resiliency through redundant DNS services, recursive query resolution, and failover support for the primary infrastructure environment.

This architecture helps eliminate single points of failure while improving long-term operational stability and network reliability.

---

## Infrastructure Role

`redundant-net` functions as the secondary infrastructure redundancy node within the environment.

Primary responsibilities include:

- Secondary DNS resolution
- Recursive DNS redundancy
- Failover support
- Infrastructure resiliency
- Service continuity during outages
- Redundant recursive query handling

The node operates alongside `infra-hub` to maintain DNS availability across the infrastructure environment.

---

## Infrastructure Architecture

| Node | Role | Services |
|------|------|-----------|
| `infra-hub` | Primary Infrastructure Node | Pi-hole, Unbound, Samba, Glances |
| `redundant-net` | Secondary Redundancy Node | Unbound, DNS Failover |

---

## Deployment Objectives

- Eliminate single points of failure
- Maintain DNS availability during outages
- Improve infrastructure resiliency
- Support recursive DNS redundancy
- Enable scalable infrastructure expansion
- Improve operational continuity across infrastructure services

---

## Recursive DNS Redundancy

The infrastructure environment utilizes Unbound recursive DNS services across both infrastructure nodes.

This configuration provides:

- Independent recursive DNS resolution
- Reduced reliance on external DNS providers
- Improved DNS privacy
- Enhanced fault tolerance
- Infrastructure-level DNS continuity

---

## Failover Design

The environment is designed to continue DNS operations even if the primary infrastructure node becomes unavailable.

### Redundancy Strategy

- `infra-hub` functions as the primary DNS infrastructure node
- `redundant-net` functions as the secondary failover node
- Recursive DNS services remain available during node interruptions
- Client systems maintain DNS functionality through secondary resolution paths

---

## Validation & Testing

The following validation procedures were performed:

- Verified recursive DNS functionality on both nodes
- Confirmed client DNS resolution continuity
- Simulated infrastructure interruption scenarios
- Tested secondary node failover behavior
- Verified service recovery after node restoration
- Confirmed infrastructure resilience during connectivity interruptions

---

## Operational Observations

Testing demonstrated that DNS services remained operational during simulated infrastructure interruptions.

During cable management and hardware adjustments, the secondary redundancy node temporarily disconnected from the network, unintentionally validating failover behavior and infrastructure resiliency within the environment.

This event confirmed successful redundancy functionality and reinforced the importance of infrastructure failover planning.

---

## Infrastructure Integration

The redundancy architecture integrates with additional infrastructure modules within the `infra-ops` ecosystem, including:

- `pihole-setup`
- `unbound`
- `glances-monitoring`
- `samba-nas`

Together, these services establish a modular infrastructure platform focused on resiliency, observability, and operational continuity.

---

## Future Improvements

Planned future enhancements include:

- Automated DNS synchronization
- Configuration replication between nodes
- Centralized monitoring integration
- Infrastructure alerting
- Containerized service deployment
- Automated failover testing
- Infrastructure diagram integration
- Expanded multi-node redundancy architecture

---

## Operational Notes

The implementation of `redundant-net` marked the transition from single-node service deployment into infrastructure resiliency engineering within the `infra-ops` environment.

This architecture established the foundation for future high-availability infrastructure expansion, operational redundancy, and scalable infrastructure design.
