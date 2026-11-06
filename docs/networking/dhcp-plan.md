# Homelab DHCP Migration Plan

## Overview

This document outlines a proposed migration from router-managed
DHCP to homelab-managed DHCP services.

The migration has not been implemented.

The objective is to improve DHCP management, DNS integration,
device consistency, and future infrastructure redundancy.

Actual network assignments and device inventories remain private.

## Current Infrastructure

The current environment includes:

- Router-managed DHCP for regular network clients
- Two Linux infrastructure nodes with static netplan addresses
- Pi-hole DNS services on both infrastructure nodes
- Existing host firewall protections
- Monitoring and infrastructure validation services

The infrastructure nodes do not depend on DHCP leases
for their primary network addresses.

## Migration Objectives

- Centralize management of DHCP reservations
- Integrate client naming with Pi-hole DNS
- Improve network troubleshooting
- Support predictable infrastructure addressing
- Develop a reliable recovery and redundancy strategy
- Preserve compatibility with existing services

Detailed device mappings and address allocations are
maintained in private operational documentation.

## Candidate DHCP Solutions

### Option 1: Pi-hole DHCP

Advantages:

- Integrates with the existing DNS infrastructure
- Provides a familiar management interface
- Requires minimal additional infrastructure

Considerations:

- Backup and configuration management
- DHCP lease and reservation handling
- Safe recovery if the primary node becomes unavailable
- Avoiding conflicting DHCP servers

### Option 2: Kea DHCP

Advantages:

- Dedicated DHCP service
- Flexible configuration management
- Potential for advanced redundancy designs
- Suitable for future automation

Considerations:

- Additional service complexity
- Firewall and network integration
- Configuration management requirements
- High-availability design and testing

### Option 3: ISC DHCP Server

A traditional Linux DHCP implementation.

Considerations:

- Familiar configuration-based management
- Legacy software lifecycle
- Limited suitability for new long-term deployments
- Separate redundancy planning

## Preferred Initial Direction

Pi-hole DHCP is the initial candidate for evaluation
because Pi-hole is already deployed.

A primary DHCP service could run on the primary
infrastructure node.

The secondary node could support a future recovery
or redundancy strategy.

A second DHCP server must not be enabled without
a validated design that prevents conflicting leases.

## Network Planning

The final DHCP configuration must define:

- Appropriate dynamic address pool
- Reserved and statically assigned addresses
- Gateway and DNS information
- Lease duration and renewal behavior
- Reservation and device naming conventions
- Backup and recovery procedures

Exact address ranges, subnets, and device assignments
are deliberately excluded from this public document.

## Firewall Requirements

DHCP relies on UDP ports 67 and 68.

Migration planning must account for:

- DHCP discovery and broadcast traffic
- Server-to-client responses
- Existing host firewall restrictions
- Required router configuration changes
- DNS and Docker networking dependencies

Firewall changes must be tested before migration.

## Pre-Migration Checklist

- Capture the existing DHCP configuration
- Inventory current reservations privately
- Verify static infrastructure addressing
- Evaluate the selected DHCP solution
- Back up relevant service configurations
- Review firewall requirements
- Select an appropriate maintenance window
- Prepare and verify a rollback procedure

## Proposed Migration Procedure

1. Back up existing DHCP and DNS configurations.
2. Confirm the final DHCP design and address pool.
3. Prepare the new DHCP service without enabling it.
4. Review firewall and network dependencies.
5. Prepare the router DHCP shutdown.
6. Enable the selected DHCP provider during maintenance.
7. Renew DHCP on a test client.
8. Confirm correct gateway and DNS assignment.
9. Verify internet connectivity and DNS resolution.
10. Confirm reservations and important client connectivity.
11. Validate monitoring and infrastructure services.
12. Document the verified operational outcome privately.
13. Publish a sanitized implementation summary.

These steps are planning guidance and must be reviewed
before any production execution.

## Rollback Procedure

If migration validation fails:

1. Disable the newly enabled DHCP service.
2. Restore the previous DHCP provider.
3. Renew leases on affected test clients.
4. Verify gateway and DNS assignment.
5. Confirm internet and internal service connectivity.
6. Restore previous firewall configuration if necessary.
7. Record the failure and required corrective actions.

Avoid leaving conflicting DHCP authorities active.

## Current Status

DHCP migration remains in the planning phase.

Both infrastructure nodes have static local addresses.

The existing router remains responsible for DHCP.

No production DHCP migration has been performed.

## Documentation Security

Operational addressing, device mappings, MAC addresses,
reservations, and router configuration remain private.

See docs/security/public-repository-policy.md for
the public repository security requirements.
