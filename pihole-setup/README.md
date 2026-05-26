# Pi-hole Infrastructure Deployment

## Overview

This module documents the deployment and configuration of Pi-hole on Ubuntu Server as part of the larger `infra-ops` infrastructure environment.

Pi-hole functions as the primary DNS filtering service within the network, providing centralized ad-blocking, local DNS management, and improved visibility into network traffic.

The service is hosted on the primary infrastructure node:

- Hostname: `infra-hub`
- Platform: Ubuntu Server
- Role: Primary DNS Filtering & Infrastructure Services Node

---

## Infrastructure Objectives

- Deploy a stable DNS filtering solution for the local network
- Establish centralized DNS visibility and management
- Enable secure remote administration through SSH
- Prepare the environment for recursive DNS integration using Unbound
- Support future redundancy and failover architecture through `redundant-net`

---

## System Installation

Ubuntu Server was deployed as the foundational operating system for the infrastructure environment.

### Initial Configuration Tasks

- Installed Ubuntu Server
- Configured static network addressing
- Enabled OpenSSH for remote management
- Updated system packages and repositories
- Verified network connectivity and DNS resolution

### Network Configuration

| Component | Value |
|---|---|
| Hostname | `infra-hub` |
| Static IP | `<infra-hub-ip>` |
| DNS Role | Primary DNS Filtering Server |
| Remote Access | OpenSSH |

---

## Pi-hole Deployment

Pi-hole was installed directly on Ubuntu Server using the official installation method.

### Installation Objectives

- Centralized DNS filtering
- Network-wide advertisement filtering
- DNS request visibility
- Simplified network-wide DNS management

### Core Features Enabled

- DNS query logging
- Web administration interface
- Local DNS management
- Blocklist integration
- Network-wide advertisement filtering

---

## Validation & Testing

The following validation steps were performed after deployment:

- Verified DNS resolution functionality
- Confirmed network-wide client connectivity
- Validated SSH remote administration access
- Confirmed successful Pi-hole web interface access
- Tested DNS filtering behavior across network devices

---

## Infrastructure Integration

This deployment serves as the foundational DNS layer for additional infrastructure services within the `infra-ops` ecosystem.

Integrated infrastructure modules include:

- `unbound`
- `redundant-net`
- `glances-monitoring`
- `samba-nas`

---

## Future Improvements

Planned future enhancements include:

- Recursive DNS integration using Unbound
- Secondary DNS failover architecture
- Additional monitoring and alerting
- Infrastructure diagram integration
- Service redundancy validation testing

---

## Operational Notes

This deployment marked the transition from isolated service experimentation into structured infrastructure operations and centralized service management.

The environment continues to evolve through modular infrastructure expansion, operational standardization, and documentation-driven development.
