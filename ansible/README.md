# Ansible Infrastructure Automation

## Overview

This directory contains Ansible playbooks for maintenance,
monitoring, and validation of a two-node Linux homelab.

Ansible runs from a Linux control workstation and connects
to managed infrastructure nodes over SSH.

## Architecture

| Component         | Role                               |
| ----------------- | ---------------------------------- |
| Linux workstation | Ansible control node               |
| Primary server    | Core infrastructure services       |
| Secondary server  | Redundancy and supporting services |

Actual network addresses and SSH identities remain private.

## Inventory Management

Public example:
`ansible/inventory/homelab.example.ini`

Private operational inventory:
`ansible/inventory/homelab.ini`

The operational inventory is excluded from Git tracking.

For a new installation, copy the public example to the private
inventory path and supply your own connection settings.

Never overwrite an existing operational inventory without
verifying its contents first.

## Playbooks

### Connectivity Test

File: `ansible/playbooks/ping.yml`

Tests Ansible connectivity to managed nodes.

Run from the repository root:

    ansible-playbook -i ansible/inventory/homelab.ini ansible/playbooks/ping.yml --ask-pass

### Maintenance Check

File: `ansible/playbooks/maintenance-check.yml`

Runs read-only infrastructure validation checks, including:

- Host identification and uptime
- Failed systemd services
- Disk and storage mount status
- Docker service and container status
- Chrony time synchronization and sources

Run from the repository root:

    ansible-playbook -i ansible/inventory/homelab.ini ansible/playbooks/maintenance-check.yml --ask-pass

## Validation History

The maintenance playbook has previously completed successfully
on both infrastructure nodes.

Recorded results per node:

- 10 successful tasks
- 0 changed tasks
- 0 unreachable hosts
- 0 failed tasks

These are historical results, not current health measurements.

## Operational Principles

Playbooks are intended to support repeatable, validated
infrastructure operations.

Production changes must be tested before being incorporated
into automation.

Future automation may include package checks, post-reboot
validation, DNS testing, storage checks, and service health.

## Repository Security

Never publish real network addresses, operational SSH identities,
credentials, private inventories, VPN configurations, or
personally identifiable device information.

Use sanitized examples and generic identifiers.

See `docs/security/public-repository-policy.md` for the
complete security requirements.

## Documentation Standards

Public documentation must reflect validated production behavior.

Review all changes before committing and pushing to GitHub.
