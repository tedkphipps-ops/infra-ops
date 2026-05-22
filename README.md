# Infrastructure_Hub_Server

A centralized, Linux-based home lab ecosystem leveraging Ubuntu Server as the foundational infrastructure layer. This repository serves as the central hub for network security, self-hosted services, and high-availability systems.

## Core Architecture
This lab is built on a mission-critical mindset, focusing on redundancy, security, and scalable infrastructure.
* **Operating System:** Ubuntu Server (Hardened for stability and security).
* **Network Topology:** Multi-node architecture ensuring zero-downtime DNS resolution.
* **Service Deployment:** Containerized and modularized services for easy maintenance and rapid recovery.

## Project Directory
* **[Redundant_Network](Redundant_Network/README.md):** High-availability DNS architecture using Pi-hole and Unbound.
* **[Pi-hole_Setup](Pi-hole_Setup/):** Configuration documentation for primary DNS filtering.
* **[Unbound](Unbound/):** Recursive DNS resolution setup.

## Technical Objectives
* **Reliability:** Maintaining 99.9% uptime for core network services.
* **Security:** Implementing proactive threat mitigation and secure access protocols.
* **Standardization:** Documenting configurations to enable rapid, repeatable deployments in any environment.

---
*Maintained by: Ted Phipps | Aspiring Data Center Technician*
