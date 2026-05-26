# infra-ops

A centralized, Linux-based infrastructure operations repository focused on resilient network services, self-hosted infrastructure, operational observability, and scalable systems administration.

This repository documents the deployment, configuration, validation, and operational evolution of the `infra-ops` infrastructure environment.

---

## Overview

The `infra-ops` environment is built around a modular infrastructure philosophy emphasizing:

- Infrastructure resiliency
- Operational continuity
- Recursive DNS independence
- Centralized infrastructure services
- Monitoring and observability
- Documentation-driven operations
- Scalable infrastructure standardization

The environment leverages Ubuntu Server as the foundational infrastructure platform supporting DNS services, monitoring, network storage, and redundancy-focused architecture.

---

## Infrastructure Architecture

The infrastructure environment currently operates across primary and secondary infrastructure nodes:

| Node | Role |
|------|------|
| `infra-hub` | Primary Infrastructure Node |
| `redundant-net` | Secondary Redundancy & Failover Node |

The environment integrates multiple infrastructure layers including:

- DNS filtering and recursive DNS services
- Infrastructure redundancy and failover
- Centralized storage services
- Infrastructure monitoring and observability
- Operational troubleshooting and validation workflows

---

## Infrastructure Modules

| Module | Purpose |
|--------|---------|
| `pihole-setup` | DNS filtering infrastructure and network-wide advertisement filtering |
| `unbound` | Recursive DNS infrastructure and private DNS query resolution |
| `redundant-net` | Infrastructure redundancy and DNS failover architecture |
| `glances-monitoring` | Infrastructure monitoring and operational observability |
| `samba-nas` | Centralized network storage and persistent infrastructure services |

---

## Operational Objectives

The infrastructure environment is designed around the following operational goals:

- Improve infrastructure resiliency
- Reduce single points of failure
- Maintain operational continuity
- Establish recursive DNS independence
- Improve infrastructure observability
- Standardize infrastructure documentation
- Support scalable infrastructure expansion
- Develop modular infrastructure services

---

## Infrastructure Philosophy

This repository emphasizes documentation-driven infrastructure operations, operational validation, iterative infrastructure refinement, and resiliency-focused systems administration.

The environment continues to evolve through structured deployment practices, infrastructure standardization, troubleshooting validation, and modular service integration.

---

## Validation & Operational Practices

Operational workflows within the environment include:

- Service validation testing
- Recursive DNS verification
- Infrastructure failover testing
- Monitoring integration
- Operational troubleshooting documentation
- Configuration standardization
- Incremental infrastructure refinement

---

## Future Roadmap

Planned future infrastructure initiatives include:

- Infrastructure diagram integration
- Expanded redundancy architecture
- Centralized backup strategies
- Infrastructure monitoring expansion
- Containerized service deployment
- Infrastructure-as-code workflows
- Automated deployment standardization
- Centralized operational documentation
- Expanded observability and alerting

---

## Documentation Expansion

Future documentation growth within the repository may include:

- `/docs/network-diagrams`
- `/docs/troubleshooting`
- `/docs/workflows`
- `/docs/infrastructure-evolution`
- `/docs/screenshots`

These additions will further support operational consistency, infrastructure scalability, and long-term documentation maintainability.

---

## Operational Notes

The `infra-ops` repository documents the ongoing evolution from isolated self-hosted services into a structured infrastructure operations environment focused on resiliency, observability, and scalable infrastructure management.

This repository serves as both an operational reference and a continuously evolving infrastructure engineering knowledge base.

---

Maintained by: Ted Phipps | Infrastructure & Systems Operations
