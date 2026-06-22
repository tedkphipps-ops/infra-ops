# Unbound Recursive DNS Infrastructure

## Overview

This module documents the deployment, configuration, and operational integration of Unbound recursive DNS services within the `infra-ops` infrastructure environment.

Unbound provides private recursive DNS resolution supporting infrastructure independence, enhanced DNS privacy, reduced reliance on third-party DNS providers, and improved operational resiliency across the homelab ecosystem.

The service operates across both infrastructure nodes and functions as the recursive backend for Pi-hole DNS filtering services.

---

## Infrastructure Role

Unbound functions as the recursive DNS resolution layer within the `infra-ops` environment.

Primary responsibilities include:

* Recursive DNS query resolution
* Independent DNS infrastructure operation
* DNS privacy enhancement
* Reduced external DNS dependency
* Infrastructure DNS resiliency
* Redundant recursive query handling
* Recursive DNS caching and performance optimization

Unbound integrates directly with Pi-hole to provide filtered DNS services backed by private recursive resolution.

---

## Infrastructure Architecture

| Node            | Role                          | Services                                                                               |
| --------------- | ----------------------------- | -------------------------------------------------------------------------------------- |
| `infra-hub`     | Primary Infrastructure Node   | Pi-hole, Unbound, Samba NAS, Glances, Grafana, Prometheus, Loki, Promtail, Uptime Kuma |
| `redundant-net` | Secondary Infrastructure Node | Pi-hole, Unbound, Samba NAS, Glances, Grafana, Prometheus, Loki, Promtail              |

---

## Deployment Objectives

* Deploy private recursive DNS infrastructure
* Improve DNS privacy and independence
* Reduce reliance on third-party DNS providers
* Support redundant DNS resolution
* Improve infrastructure resiliency
* Establish scalable recursive DNS architecture
* Create a foundation for long-term DNS observability

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

```yaml
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

Enable and start Unbound:

```bash
sudo systemctl enable unbound
sudo systemctl restart unbound
```

---

## Pi-hole Integration

Pi-hole forwards DNS requests to the local Unbound recursive resolver.

Example Pi-hole forwarding configuration:

```text
127.0.0.1#5335
```

This integration allows Pi-hole to provide DNS filtering while Unbound performs secure recursive query resolution.

---

## Validation & Testing

The following validation procedures were performed:

* Verified recursive DNS query resolution
* Confirmed Unbound service startup
* Validated Pi-hole integration
* Confirmed local recursive DNS functionality
* Tested DNS resolution continuity across infrastructure nodes
* Verified recursive query responses during failover testing

Verify service status:

```bash
systemctl status unbound
```

Test recursive resolution:

```bash
dig google.com @127.0.0.1 -p 5335
```

Validate local DNS functionality:

```bash
nslookup openai.com 127.0.0.1
```

---

## Recursive DNS Benefits

The deployment of Unbound provides several operational advantages:

* Increased DNS privacy
* Independent recursive DNS control
* Reduced dependency on external DNS providers
* Improved DNS query integrity
* Recursive caching improvements
* Infrastructure-level DNS resiliency
* Improved visibility into DNS operations

---

## Monitoring & Validation

Recursive DNS operations are monitored through:

* Pi-hole query metrics
* Grafana dashboards
* Prometheus metric collection
* Uptime Kuma service monitoring

These platforms provide operational visibility into DNS availability, query activity, service health, and infrastructure status.

---

## Infrastructure Integration

Unbound integrates with additional infrastructure modules throughout the `infra-ops` ecosystem:

* `pihole-setup`
* `redundant-net`
* `glances-monitoring`
* `samba-nas`
* `monitoring`

Together these services establish a resilient infrastructure platform supporting DNS services, monitoring, storage services, and operational continuity.

---

## Future Improvements

Planned future enhancements include:

* Additional DNS resiliency testing
* Expanded failover validation procedures
* Future VLAN and subnet DNS segmentation
* Additional recursive DNS monitoring
* Infrastructure-wide alerting integration
* Long-term DNS analytics and reporting
* Continued documentation standardization

---

## Operational Notes

The implementation of Unbound introduced fully recursive DNS resolution into the homelab environment and eliminated reliance on third-party public DNS providers for recursive query resolution.

Unbound operates as the recursive backend for both Pi-hole instances and serves as a foundational component of the infrastructure DNS architecture.

The deployment established the basis for resilient DNS operations, privacy-focused name resolution, and future infrastructure growth across both infrastructure nodes.

A significant operational milestone during deployment included resolving trust-anchor configuration issues and validating recursive resolution functionality through Pi-hole integration and redundancy testing.

The Unbound deployment remains a core component of the infrastructure design and continues to provide private, recursive DNS services across the homelab ecosystem.
