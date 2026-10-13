# DHCP Migration Plan

This document plans a future DHCP migration for the homelab network.

No production DHCP changes have been made as part of this document.

## Current State

The Spectrum router currently provides DHCP for normal LAN clients.

Core infrastructure nodes have been moved to true static local netplan addresses before DHCP migration work.

| Item | Current Value |
|---|---|
| Router | `192.168.1.1` |
| LAN subnet | `192.168.1.0/24` |
| Current DHCP provider | Spectrum router for regular clients |
| Static infrastructure nodes | HUB and RN |
| Current DNS services | Pi-hole on HUB and RN |

## Core Infrastructure Addresses

These addresses are the stable baseline before any DHCP migration.

HUB and RN are configured with static local netplan addresses, not DHCP leases.

| Device | Hostname / Role | IP |
|---|---|---|
| Router | Spectrum router | `192.168.1.1` |
| HUB | `infra-hub` | `192.168.1.225` |
| RN | `redundant-net` | `192.168.1.237` |
| Lenovo | WSL/admin workstation host | `192.168.1.250` |

## Migration Goal

Move DHCP control away from the Spectrum router and into homelab-managed infrastructure.

Primary goals:

| Goal | Reason |
|---|---|
| Controlled DHCP reservations | Keep important devices predictable |
| DNS integration | Hand out Pi-hole DNS cleanly |
| Better documentation | Track device identity and IP intent |
| Easier troubleshooting | Keep network state visible in the infra repo |
| Future redundancy | Reduce dependence on one unmanaged router feature set |

## Candidate DHCP Approaches

### Option 1: Pi-hole DHCP

Pi-hole DHCP is the simplest candidate because Pi-hole is already deployed on both HUB and RN.

Potential benefits:

| Benefit | Notes |
|---|---|
| Simple UI | DHCP can be managed from Pi-hole admin |
| DNS integration | DHCP clients can register names with Pi-hole |
| Existing service | No new DHCP package required if Pi-hole DHCP is suitable |

Concerns to validate:

| Concern | Notes |
|---|---|
| Redundancy | Need to avoid two active DHCP servers handing out conflicting leases |
| Backup behavior | Need a safe failover plan if HUB is down |
| Config sync | Need to understand how DHCP reservations are stored and backed up |

### Option 2: Kea DHCP

Kea is a modern DHCP server option.

Potential benefits:

| Benefit | Notes |
|---|---|
| Purpose-built DHCP | More advanced DHCP service than Pi-hole UI |
| Automation-friendly | Better long-term config-as-code option |
| Possible HA options | Worth researching before implementation |

Concerns to validate:

| Concern | Notes |
|---|---|
| More complexity | More moving parts than Pi-hole DHCP |
| Requires new service | Adds another component to maintain |
| Needs careful firewall rules | DHCP traffic must be explicitly allowed |

### Option 3: isc-dhcp-server

Traditional Linux DHCP server.

Potential benefits:

| Benefit | Notes |
|---|---|
| Familiar | Common Linux DHCP implementation |
| Simple static config | Reservations can be tracked in config files |

Concerns to validate:

| Concern | Notes |
|---|---|
| Older path | May not be the best long-term choice |
| Manual management | Less integrated with current Pi-hole setup |
| HA planning needed | Redundancy still needs design work |

## Preferred Initial Direction

Preferred first candidate for lab evaluation:

| Choice | Reason |
|---|---|
| Pi-hole DHCP on HUB | Lowest-complexity first step because Pi-hole is already deployed |

RN should not be enabled as a second active DHCP server without a clear failover design.

## Proposed Future State

Initial target:

| Role | Node | Notes |
|---|---|---|
| Primary DHCP | HUB / `infra-hub` | Main DHCP service |
| DNS | HUB and RN Pi-hole | DHCP clients should receive Pi-hole DNS |
| Failover / backup | RN / `redundant-net` | Planned but not enabled until safe design exists |
| Router | Spectrum router | Gateway only, DHCP disabled after migration |

## Draft DHCP Scope

Exact pool must be verified before implementation.

Proposed concept:

| Range | Purpose |
|---|---|
| `192.168.1.2` - `192.168.1.99` | General dynamic clients |
| `192.168.1.100` - `192.168.1.199` | Smart TVs, phones, IoT, general reservations |
| `192.168.1.200` - `192.168.1.254` | Infrastructure, admin, known fixed/reserved devices |

Known infrastructure should stay outside normal dynamic assignment.

## Known Important Addresses

| Device / Role | IP | Notes |
|---|---|---|
| Router | `192.168.1.1` | Gateway |
| HUB | `192.168.1.225` | Static local netplan address; core services |
| RN | `192.168.1.237` | Static local netplan address; redundant services |
| Lenovo admin workstation | `192.168.1.250` | Admin source for firewall and Ansible |
| Brother printer | `192.168.1.211` | Known printer |
| PlayStation 5 | `192.168.1.86` | Known device |
| Smart frame | `192.168.1.223` | Known device |
| Insignia Roku TV | `192.168.1.96` | Known Roku device |
| Little TV Roku | `192.168.1.200` | Known Roku device |
| Hisense Roku TV | `192.168.1.222` | Known Roku device |

## Firewall Considerations

Current firewall rules allow DHCP replies from the current router.

Before changing DHCP providers, firewall rules must be reviewed and updated.

Items to validate:

| Item | Requirement |
|---|---|
| DHCP server traffic | UDP `67/68` must work correctly |
| Client broadcasts | DHCP discovery must reach the DHCP server |
| Router DHCP rule | Existing router DHCP safety rule may need adjustment |
| HUB/RN firewall | DHCP service must not be blocked locally |
| Docker interaction | DHCP should not conflict with Docker networking |

## Pre-Migration Checklist

Before disabling DHCP on the router:

| Check | Status |
|---|---|
| Current router DHCP settings captured | Pending |
| Current DHCP range captured | Pending |
| Current reservations captured | Pending |
| Pi-hole DHCP capability inspected | Pending |
| HUB/RN Pi-hole config backed up | Pending |
| Firewall DHCP rules reviewed | Pending |
| Rollback plan written | Pending |
| Maintenance window chosen | Pending |

## Migration Draft Steps

Do not run these until the plan is validated.

1. Capture router DHCP settings and screenshots.
2. Export or document existing DHCP reservations.
3. Back up HUB and RN Pi-hole configuration.
4. Decide DHCP provider and lease range.
5. Configure DHCP on chosen provider but do not enable yet.
6. Update firewall rules if needed.
7. Disable DHCP on Spectrum router.
8. Enable DHCP on chosen homelab provider.
9. Renew DHCP on a test client.
10. Confirm client receives correct gateway and DNS.
11. Confirm internet access.
12. Confirm Pi-hole query visibility.
13. Confirm important devices retain expected IPs.
14. Document final configuration.
15. Commit and push documentation.

## Rollback Plan

If migration fails:

1. Disable homelab DHCP service.
2. Re-enable DHCP on Spectrum router.
3. Reboot or renew DHCP on affected clients.
4. Confirm clients receive router DHCP leases again.
5. Confirm DNS and internet access.
6. Restore previous firewall rules if they were changed.

## Current Status

Planning only.

HUB and RN have been moved to static local netplan addresses.

No production DHCP authority migration has been made.
