# Homelab Firewall Architecture and Hardening

## Overview

This document describes the design, implementation, and
validation of host firewall protections across a two-node
Linux homelab.

The objective is to restrict infrastructure management access
while preserving DNS, storage, monitoring, and other required
services.

Operational firewall rules and network details remain private.

## Security Objectives

- Restrict administrative interfaces to trusted clients.
- Preserve required infrastructure services.
- Protect monitoring and management endpoints.
- Support necessary communication between infrastructure nodes.
- Restrict unnecessary access from untrusted devices.
- Preserve SSH connectivity during firewall maintenance.
- Maintain persistent filtering across system restarts.

## Firewall Architecture

Both infrastructure nodes use:

- iptables for IPv4 packet filtering
- ip6tables for IPv6 packet filtering
- netfilter-persistent for rule persistence
- Docker-aware filtering for published container services

The initial firewall design considered UFW.

During implementation, direct iptables management was adopted
after package changes removed UFW.

## Host Firewall Design

The IPv4 host firewall follows these principles:

1. Permit localhost traffic.
2. Permit established and related connections.
3. Allow required infrastructure services.
4. Restrict management access to authorized sources.
5. Permit necessary infrastructure communication.
6. Drop other inbound traffic.

The specific source addresses, interfaces, and port
assignments are maintained in private operational records.

## Docker Firewall Integration

Docker manages networking rules for published container ports.

Host INPUT filtering alone may not protect Docker-published
services because of Docker's forwarding behavior.

The implementation uses the DOCKER-USER chain to apply
additional access restrictions to container services.

This protects monitoring and administrative interfaces
while preserving required service communication.

IPv6 filtering is also applied to selected published
services to prevent unintended access paths.

## Rule Persistence

Firewall configuration is maintained through
netfilter-persistent.

The implementation includes:

- Persistent IPv4 filtering
- Persistent IPv6 filtering
- Docker-specific access controls
- Configuration backups
- Rule validation before applying changes

Changes must be reviewed and tested before being considered
production-ready.

## Implementation History

### October 2026 - Initial Hardening

Host and Docker firewall protections were implemented
and validated on both infrastructure nodes.

The implementation included:

- Administrative access restrictions
- Infrastructure service exceptions
- Docker-published service filtering
- IPv6 access restrictions for selected services
- Persistent firewall configuration
- Recovery and rollback checkpoints

### October 31, 2026 - Monitoring Recovery

A monitoring exporter experienced a connectivity failure
because hardened host filtering blocked necessary
container-to-host communication.

Troubleshooting identified the affected traffic path.

A targeted firewall exception restored connectivity
without removing the broader inbound filtering policy.

Validation confirmed:

- Required application API connectivity restored
- Exporter metrics returned HTTP 200
- Firewall rule syntax validation passed
- Persistent rules were updated
- Monitoring functionality recovered

The correction was also verified following a reboot.

## Validation Methods

Firewall maintenance uses several validation methods:

- SSH connectivity checks
- DNS resolution tests
- Infrastructure service health checks
- Monitoring endpoint availability tests
- Docker service connectivity tests
- Firewall rule and packet counter inspection
- IPv4 ruleset syntax validation using `iptables-restore --test`
- Persistent rule validation using `netfilter-persistent`
- Post-reboot verification

Historical validation confirmed required management and
infrastructure connectivity from authorized systems.

These tests do not guarantee that every possible
unauthorized access path has been tested.

## Operational Safety

Before making firewall changes:

1. Preserve a known-good configuration.
2. Confirm administrative access.
3. Review service dependencies.
4. Prepare a rollback procedure.
5. Apply changes to one node at a time.
6. Validate connectivity and service health.
7. Verify persistence after changes.
8. Record the completed maintenance work.

## Security and Documentation

Detailed firewall rules, trusted addresses, interface
information, and operational recovery records are private.

Public documentation describes the architecture, reasoning,
implementation process, and verified outcomes.

See docs/security/public-repository-policy.md for
the public documentation requirements.

## Status

Firewall hardening has been implemented and historically
validated on both infrastructure nodes.

Current production status must be confirmed through
live operational checks when required.
