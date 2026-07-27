# Ansible Infrastructure Automation

## Overview

This directory contains Ansible inventory and playbooks used to automate maintenance checks and validation tasks across the `infra-ops` homelab environment.

Ansible is run from the Lenovo WSL Ubuntu command environment and connects to infrastructure nodes over SSH.

## Control Node

| System | Role |
| --- | --- |
| Lenovo WSL Ubuntu | Ansible control node |
| `infra-hub` | Managed infrastructure node |
| `redundant-net` | Managed infrastructure node |

## Managed Nodes

| Host | IP Address | Role |
| --- | --- | --- |
| `infra-hub` | `192.168.1.225` | Primary Infrastructure Node |
| `redundant-net` | `192.168.1.237` | Secondary Infrastructure Node |

## Directory Structure

```text
ansible/
├── README.md
├── inventory/
│   └── homelab.ini
└── playbooks/
    ├── ping.yml
    └── maintenance-check.yml
```

## Inventory

The inventory file is located at:

`ansible/inventory/homelab.ini`

The inventory defines the managed homelab nodes and the SSH user used by Ansible.

Current managed group:

```ini
[homelab]
infra-hub ansible_host=192.168.1.225
redundant-net ansible_host=192.168.1.237

[homelab:vars]
ansible_user=ted_phipps
ansible_python_interpreter=/usr/bin/python3
```

## Playbooks

### ping.yml

File location:

`ansible/playbooks/ping.yml`

Purpose:

Validates that Ansible can connect to both managed nodes over SSH.

Run from the repository root with:

```bash
ansible-playbook -i ansible/inventory/homelab.ini ansible/playbooks/ping.yml --ask-pass
```

Expected result:

Both `infra-hub` and `redundant-net` should return successful Ansible ping results.

### maintenance-check.yml

File location:

`ansible/playbooks/maintenance-check.yml`

Purpose:

Runs read-only maintenance checks across both infrastructure nodes.

Current checks include:

- Hostname validation
- Uptime review
- Failed systemd service review
- Disk and NAS mount validation
- Docker service status
- Docker container listing
- Chrony tracking
- Chrony source validation

Run from the repository root with:

```bash
ansible-playbook -i ansible/inventory/homelab.ini ansible/playbooks/maintenance-check.yml --ask-pass
```

## Current Validation Status

The Ansible maintenance check has been validated against both infrastructure nodes.

Current successful result:

```text
infra-hub      ok=10 changed=0 unreachable=0 failed=0
redundant-net  ok=10 changed=0 unreachable=0 failed=0
```

Validated production checks include:

- `infra-hub` reachable by Ansible
- `redundant-net` reachable by Ansible
- Docker service active on both nodes
- Docker containers visible to the Ansible SSH user
- `/mnt/hub-nas` mounted on `infra-hub`
- `/mnt/rn-nas` mounted on `redundant-net`
- Chrony synchronized on both nodes
- Leap status normal on both nodes

## Docker Access

The Ansible SSH user has been added to the Docker group on both managed nodes so Docker status and container checks can run without sudo.

This allows the maintenance playbook to validate running containers as part of the standard read-only maintenance check.

## Known Service Notes

The following failed services may appear during maintenance checks:

- `openipmi.service`
- `unbound-resolvconf.service`

These should be reviewed during maintenance but are currently known observations in the environment.

## Operational Use

This Ansible setup supports the documented `system-maintenance` workflow by converting manual checks into repeatable automation.

The current playbooks are intentionally read-only and are used for validation, visibility, and operational consistency.

Future playbooks may include:

- Package update checks
- Post-reboot validation
- DNS validation
- Storage validation
- Service-specific health checks
- Controlled maintenance workflows

## Notes

Ansible automation should reflect the real production state of the homelab.

Production changes should be validated manually first, then converted into repeatable Ansible playbooks after the workflow is proven.
