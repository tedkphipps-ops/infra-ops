# Infrastructure Documentation

This directory contains centralized documentation for the infra-ops homelab project.

Documentation is organized by functional area to provide operational references, architecture diagrams, monitoring information, networking documentation, and infrastructure guidance.

## Documentation Structure

### diagrams/

Infrastructure architecture and visual references.

Contents include:

* Network topology diagrams
* DNS flow diagrams
* Service dependency diagrams

See: `docs/diagrams/README.md`

### monitoring/

Monitoring platform documentation and dashboard references.

Contents include:

* Uptime Kuma monitoring configuration
* Status page documentation
* Monitoring screenshots
* Service availability references

See: `docs/monitoring/README.md`

### networking/

Network-related documentation and infrastructure references.

Contents include:

* Network architecture
* Device inventory
* Addressing references
* Connectivity documentation

See: `docs/networking/README.md`

### git-cli/

Git and command-line operational references.

Contents include:

* Git workflow documentation
* Common command references
* Repository management notes

## Homelab Overview

The infra-ops homelab consists of two primary infrastructure hosts designed to provide DNS services, monitoring, NAS storage, and operational redundancy.

### infra-hub (HUB)

Primary infrastructure node providing:

* Pi-hole
* Unbound
* Samba NAS
* hub-share (1 TB storage)
* Glances
* Uptime Kuma

### redundant-net (RN)

Secondary infrastructure node providing:

* Pi-hole
* Unbound
* Samba NAS
* backup-share (3 TB storage)
* Glances

## Core Infrastructure Services

The homelab currently provides:

* DNS filtering and recursive resolution
* DNS redundancy and failover capability
* Centralized monitoring and status reporting
* Network-attached storage (NAS)
* Infrastructure documentation and diagrams
* Service dependency tracking

## Documentation Standards

* Use descriptive filenames.
* Store diagrams in `docs/diagrams/`.
* Store monitoring screenshots in `docs/monitoring/screenshots/`.
* Avoid publishing real infrastructure IP addresses in public documentation.
* Update documentation when infrastructure changes occur.

## Notes

Documentation is updated as infrastructure services, diagrams, monitoring platforms, and homelab capabilities evolve.
