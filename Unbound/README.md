# Unbound Recursive DNS Server

This directory contains the configuration and documentation for the Unbound recursive DNS resolver. 

## Overview
Unbound is configured as a local recursive DNS resolver, providing a private, secure path for DNS queries. It is designed to work in tandem with Pi-hole to ensure that queries are resolved locally rather than relying on external providers.

## Configuration Details
- **Listen Address:** `127.0.0.1` (Localhost)
- **Port:** `5335`
- **Upstream:** Root hints (no forwarding)

## Redundancy Plan
To ensure high availability across the home lab (Primary Server + Toshiba Laptop):
1. **Consistency:** Configuration files will be mirrored across both nodes.
2. **Synchronization:** We will implement a sync strategy to ensure DNS lists and configurations remain uniform.
3. **Failover:** DNS clients will be configured to point to both server IPs to ensure uptime during maintenance or hardware failure.

## Roadmap
- [ ] Install and configure Unbound on the primary server.
- [ ] Test recursive resolution speed and privacy.
- [ ] Deploy identical Unbound configuration to secondary (Toshiba) node.
- [ ] Configure failover networking between nodes.
