# infra-ops

A centralized Linux-based infrastructure environment leveraging Ubuntu Server as the foundational platform for self-hosted services, network security, and operational infrastructure management.

## Core Architecture

This environment is designed with a focus on reliability, redundancy, security, and scalable infrastructure practices.

- Operating System: Ubuntu Server
- Network Topology: Multi-node infrastructure supporting redundant DNS services
- Service Deployment: Containerized and modularized services for maintainability and rapid recovery
- Monitoring: Lightweight infrastructure monitoring and operational visibility tools

## Project Directory

- redundant-net : Redundant DNS infrastructure using Pi-hole and Unbound
- pihole-setup : Primary DNS filtering configuration and deployment
- unbound : Recursive DNS resolution configuration
- samba-nas : Local network-attached storage configuration

## Technical Objectives

- Reliability: Maintaining stable and resilient core network services
- Security: Implementing secure administrative and network practices
- Standardization: Creating repeatable deployment and documentation workflows
- Scalability: Building modular infrastructure capable of future expansion

## Current Infrastructure

- infra-hub : Primary infrastructure node
- redundant-net : Secondary infrastructure and DNS redundancy node

Maintained by Ted Phipps | Aspiring Data Center Technician
