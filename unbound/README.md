# Unbound Recursive DNS Infrastructure

## Overview

This module documents the deployment and operational configuration of Unbound recursive DNS services within the `infra-ops` infrastructure environment.

Unbound provides private, recursive DNS resolution services supporting infrastructure independence, improved DNS privacy, reduced reliance on external DNS providers, and enhanced operational resiliency across the infrastructure environment.

The service operates across both primary and secondary infrastructure nodes to support redundant recursive DNS resolution.

---

## Infrastructure Role

Unbound functions as the recursive DNS resolution layer within the `infra-ops` environment.

Primary responsibilities include:

- Recursive DNS query resolution
- Independent DNS infrastructure operation
- DNS privacy enhancement
- Reduced external DNS dependency
- Infrastructure DNS resiliency
- Redundant recursive query handling

Unbound integrates directly with Pi-hole to provide secure and controlled recursive DNS services throughout the infrastructure environment.

---

## Infrastructure Architecture

| Node | Role | Services |
|------|------|-----------|
| `infra-hub` | Primary Infrastructure Node | Pi-hole, Unbound, Samba, Glances |
| `redundant-net` | Secondary Redundancy Node | Unbound, DNS Failover |

---

## Deployment Objectives

- Deploy private recursive DNS infrastructure
- Improve DNS privacy and independence
- Reduce reliance on third-party DNS providers
- Support redundant DNS resolution
- Improve infrastructure resiliency
- Establish scalable recursive DNS architecture

---

## Unbound Installation

### Install Unbound Services

Update system packages and install Unbound:

```bash
sudo apt update && sudo apt install unbound -y
```

---

## Unbound Configuration

Edit the primary Unbound configuration file:

```bash
sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf
```

Add the following recursive DNS configuration:

```conf
server:
    verbosity: 0
    interface: 127.0.0.1
    port: 5335
    do-ip4: yes
    do-udp: yes
    do-tcp: yes
    harden-glue: yes
    harden-dnssec-stripped: yes
    use-caps-for-id: no
    edns-buffer-size: 1232
    prefetch: yes
    num-threads: 1
    so-rcvbuf: 1m

    private-address: 192.168.0.0/16
    private-address: 172.16.0.0/12
    private-address: 10.0.0.0/8
```

---

## Service Initialization

Enable and start Unbound services:

```bash
sudo systemctl enable unbound
sudo systemctl restart unbound
```

---

## Pi-hole Integration

Pi-hole forwards DNS requests to the local Unbound recursive resolver.

### Example Pi-hole Forwarding Configuration

```text
127.0.0.1#5335
```

This integration allows Pi-hole to provide filtered DNS services while Unbound performs secure recursive query resolution.

---

## Validation & Testing

The following validation procedures were performed:

- Verified recursive DNS query resolution
- Confirmed Unbound service startup
- Validated Pi-hole integration
- Confirmed local recursive DNS functionality
- Tested DNS resolution continuity across infrastructure nodes
- Verified recursive query responses during failover testing

Verify Unbound service status:

```bash
systemctl status unbound
```

Test recursive DNS resolution:

```bash
dig google.com @127.0.0.1 -p 5335
```

Validate local DNS functionality:

```bash
nslookup openai.com 127.0.0.1
```

---

## Recursive DNS Benefits

The deployment of Unbound provides several operational advantages within the infrastructure environment:

- Increased DNS privacy
- Independent recursive DNS control
- Reduced dependency on external DNS providers
- Improved DNS query integrity
- Recursive caching improvements
- Infrastructure-level DNS resiliency

---

## Infrastructure Integration

Unbound integrates with additional infrastructure modules within the `infra-ops` ecosystem, including:

- `pihole-setup`
- `redundant-net`
- `glances-monitoring`
- `samba-nas`

Together, these services establish a resilient and modular infrastructure platform supporting operational continuity, monitoring, and infrastructure scalability.

---

## Future Improvements

Planned future enhancements include:

- DNSSEC hardening improvements
- Automated root hints updates
- Multi-node recursive DNS synchronization
- Containerized DNS deployment
- Encrypted upstream DNS integration
- Centralized infrastructure monitoring
- Expanded redundancy testing
- Infrastructure visualization integration

---

## Operational Notes

The implementation of Unbound introduced fully recursive DNS infrastructure into the `infra-ops` environment, significantly improving DNS independence, resiliency, and operational control.

This deployment established the foundation for long-term infrastructure scalability, redundant recursive resolution, and privacy-focused DNS operations across the infrastructure platform.
