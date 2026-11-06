# Infrastructure Maintenance Log

## Overview

This document records validated maintenance, recovery,
and infrastructure improvements in a two-node Linux homelab.

Entries describe completed work and historical results.

Private network settings, authentication details,
and operational configuration remain excluded.

## October 31, 2026 - Crash-Kernel Memory Recovery

### Issue

The secondary infrastructure node had limited available RAM.

Investigation identified a 320 MiB memory reservation
for crash-kernel functionality that was not being used.

### Resolution

- Confirmed kernel crash recovery was disabled.
- Removed the unused kdump-tools package.
- Removed the obsolete crash-kernel configuration.
- Regenerated GRUB using update-grub.
- Verified the generated configuration.
- Rebooted the node and performed health checks.

### Validation

- Crash-kernel reservation reduced to zero.
- Approximately 320 MiB of usable RAM recovered.
- Total usable memory increased to about 3.3 GiB.
- Essential infrastructure services remained active.
- Storage and monitoring services were verified.
- Persistent firewall configuration survived reboot.

Result: Successfully completed.

## October 31, 2026 - Monitoring Exporter Recovery

### Issue

A monitoring exporter experienced excessive memory
and swap consumption.

A separate connectivity problem prevented the exporter
from reaching its required application API.

### Resolution

- Investigated process memory and swap utilization.
- Restarted the affected exporter container.
- Identified a Docker-to-host firewall restriction.
- Applied a targeted firewall correction.
- Validated firewall syntax and persistence.
- Confirmed restored monitoring functionality.

### Validation

- Host RAM utilization decreased substantially.
- Exporter swap consumption returned to zero.
- Exporter metrics endpoint returned HTTP 200.
- Application API connectivity was restored.
- The firewall correction survived reboot.

Result: Successfully completed.

Related documentation:
[Exporter Recovery](rn-exporter-recovery-2026-10-31.md)

## October 31, 2026 - Remote Administration Validation

### Work Performed

Validated private remote administration capabilities
from an additional management workstation.

Validation included:

- Authenticated SSH connectivity
- Private networking availability
- Compatibility with existing firewall restrictions
- Infrastructure health checks
- Git and GitHub workflow availability

The established Linux Ansible control environment
remained unchanged.

Result: Remote administration successfully validated.

Related documentation:
[Secure Remote Administration](tailscale-remote-access.md)

## October 25, 2026 - NAS Filesystem Recovery

### Issue

A secondary storage device experienced an NTFS
filesystem dirty-flag condition.

The condition interfered with normal filesystem mounting.

### Resolution

- Investigated the NTFS filesystem condition.
- Cleared the filesystem dirty flag.
- Removed the temporary forced-mount configuration.
- Restored normal NTFS3 mounting.
- Validated read-write storage access.
- Confirmed Samba availability.

### Validation

Normal filesystem mounting and storage sharing
were restored successfully.

A separate USB reset investigation remained open
at the time of the recovery.

Related documentation:
[NAS Recovery](rn-nas-recovery-2026-10-25.md)

## October 12, 2026 - Static Network Configuration

### Objective

Prepare the infrastructure nodes for a future
DHCP migration by removing their dependence on
router-assigned DHCP leases.

### Work Performed

- Reviewed existing netplan configuration.
- Removed obsolete and duplicate configuration.
- Created configuration backups.
- Configured static IPv4 addressing.
- Used netplan try for safe configuration testing.
- Verified network routes and DNS resolution.
- Validated infrastructure service connectivity.

### Validation

Both infrastructure nodes retained their expected
network connectivity after conversion.

Ansible maintenance checks completed successfully
with zero failed or unreachable hosts.

Result: Static network configuration validated.

DHCP migration itself remains planned, not implemented.

## October 10, 2026 - Firewall and Kernel Maintenance

### Work Performed

- Applied host firewall hardening.
- Implemented Docker-aware traffic filtering.
- Configured persistent IPv4 and IPv6 rules.
- Prepared recovery and rollback checkpoints.
- Applied pending kernel updates.
- Rebooted infrastructure nodes individually.
- Verified service availability after each reboot.

### Validation

- SSH connectivity restored successfully.
- DNS and storage services operational.
- Docker containers running.
- Monitoring services available.
- Persistent firewall configuration verified.
- Ansible maintenance checks passed.

Result: Firewall and kernel maintenance validated.

## October 10, 2026 - Failed Service Cleanup

### Issue

An unnecessary IPMI-related service produced failed
systemd status reports on laptop-based infrastructure nodes.

### Resolution

- Investigated the failed service.
- Confirmed IPMI functionality was not required.
- Disabled and masked openipmi.service.
- Cleared its failed systemd state.
- Preserved installed monitoring-related packages.

### Validation

Both infrastructure nodes subsequently reported
zero failed systemd units.

Ansible maintenance checks completed successfully.

Result: Unnecessary service failure noise eliminated.

## Maintenance Principles

Infrastructure maintenance follows these practices:

1. Inspect current production state.
2. Identify dependencies and potential risks.
3. Preserve configuration backups.
4. Prepare recovery and rollback procedures.
5. Make controlled changes.
6. Validate functionality after changes.
7. Document verified outcomes.
8. Publish sanitized technical summaries.

## Security and Documentation

Historical maintenance records are not a substitute
for current production health checks.

Detailed configurations, private network information,
and sensitive recovery records are maintained separately.

See docs/security/public-repository-policy.md
for public documentation requirements.
