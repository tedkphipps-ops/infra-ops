# Infrastructure Diagrams

This directory contains architecture, topology, and service dependency diagrams used throughout the infra-ops homelab project.

## Contents

### net-topology.png

Current infrastructure topology diagram showing core homelab systems, monitoring services, DNS services, storage services, and relationships between infra-hub and redundant-net.

### dns-flow.png

DNS resolution and failover flow diagram documenting Pi-hole and Unbound interactions throughout the environment.

### service-dependency.png

Infrastructure service dependency diagram illustrating relationships between infrastructure services, DNS services, monitoring services, and storage resources.

## Diagram Coverage

The infrastructure diagrams document:

* Internet and router connectivity
* infra-hub (primary infrastructure node)
* redundant-net (secondary redundancy node)
* Pi-hole DNS filtering
* Unbound recursive DNS
* Glances monitoring
* NAS storage services
* Uptime Kuma monitoring
* Grafana dashboards
* Prometheus metrics collection
* Loki log aggregation
* Promtail log shipping
* Node Exporter system metrics
* Pi-hole Exporter metrics integration

## Notes

Diagrams are updated as infrastructure services evolve and major architectural changes are implemented.

Editable source files should be retained whenever possible to support future updates and documentation maintenance.
