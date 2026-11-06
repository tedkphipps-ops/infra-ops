# Unbound Recursive DNS Infrastructure

## Overview

This module documents the deployment and integration
of Unbound recursive DNS services in a two-node
Linux homelab.

Unbound provides recursive DNS resolution for Pi-hole,
reducing reliance on third-party recursive resolvers.

The architecture supports DNS filtering, caching,
privacy, and infrastructure resiliency.

## Architecture

The environment uses two infrastructure nodes.

Each node provides:

- Pi-hole DNS filtering
- Unbound recursive resolution
- Local DNS caching
- DNS service monitoring

The general DNS request path is:

    Client --> Pi-hole --> Unbound --> DNS hierarchy

Actual network addressing and configuration
remain private.

## Deployment Objectives

- Provide independent recursive DNS resolution
- Integrate Unbound with Pi-hole
- Improve DNS privacy
- Support redundant DNS infrastructure
- Improve DNS caching and performance
- Validate DNS service availability
- Support infrastructure monitoring

## Installation

Install Unbound on a supported Ubuntu system:

    sudo apt update
    sudo apt install unbound

## Example Unbound Configuration

Unbound configuration files are normally stored
under /etc/unbound/.

The following is an illustrative configuration
for a local recursive resolver.

It is not a copy of production configuration.

    server:
        verbosity: 0
        interface: 127.0.0.1
        port: 5335
        do-ip4: yes
        do-udp: yes
        do-tcp: yes
        harden-glue: yes
        harden-dnssec-stripped: yes
        edns-buffer-size: 1232
        prefetch: yes
        num-threads: 1

The loopback address restricts this example
resolver to local connections.

Production configurations may require additional
settings for DNSSEC, access control, and
environment-specific requirements.

## Pi-hole Integration

Pi-hole forwards permitted DNS queries to
the local Unbound recursive resolver.

An example local upstream destination is:

    127.0.0.1#5335

Pi-hole performs DNS filtering while Unbound
handles recursive resolution.

The actual operational DNS configuration
is maintained privately.

## Service Management

Validate the Unbound configuration:

    sudo unbound-checkconf

Enable the Unbound service:

    sudo systemctl enable unbound

Restart the service after configuration changes:

    sudo systemctl restart unbound

Inspect service health:

    systemctl status unbound

## DNS Validation

Test local recursive resolution:

    dig @127.0.0.1 -p 5335 example.org

Check the DNS response and query status.

Confirm that Pi-hole forwards permitted queries
to the configured recursive resolver.

Validate both infrastructure nodes independently.

## Monitoring

DNS operations are monitored through:

- Pi-hole query metrics
- Prometheus metrics collection
- Grafana dashboards
- Uptime Kuma availability checks

Monitoring supports investigation of DNS failures,
query activity, and infrastructure health.

## Deployment Validation

Historical deployment testing included:

- Unbound service startup
- Recursive DNS query resolution
- Pi-hole upstream integration
- DNS functionality across both infrastructure nodes
- Recursive resolver troubleshooting
- Trust-anchor configuration correction

These represent historical validation results,
not a guarantee of current service availability.

## Future Improvements

Potential enhancements include:

- Additional DNS resiliency testing
- Expanded failover validation
- DNS health automation through Ansible
- Additional monitoring and alerting
- Future network segmentation
- Continued configuration documentation

## Security Requirements

This public document intentionally excludes
real network addresses, operational DNS mappings,
private configuration, and sensitive access details.

See docs/security/public-repository-policy.md
for publication requirements.
