# Tailscale Remote Homelab Access

## Overview

Tailscale provides secure remote network connectivity between the ROG Zephyrus workstation and both infrastructure nodes.

This allows remote administration while away from the home LAN without exposing SSH directly through the Spectrum router.

## Connected Systems

| Device | Role | Tailscale Address |
|---|---|---|
| infra-hub | Primary infrastructure node | REDACTED_PRIVATE_IP |
| redundant-net | Secondary infrastructure node | REDACTED_PRIVATE_IP |
| rog-zephyrus | Remote administration workstation | REDACTED_PRIVATE_IP |

## Remote Administration

The ROG runs Windows PowerShell with OpenSSH for Windows.

Local Windows account:

    local_user

Remote Linux SSH account on HUB and RN:

    your_ssh_username

The ROG uses a dedicated passphrase-protected Ed25519 SSH key for homelab access.

The private key and its passphrase must never be committed to GitHub.

## Validated Access

Remote SSH connectivity from ROG to both HUB and RN was successfully tested over Tailscale while away from the home network.

Example remote connection:

    ssh your_ssh_username@REDACTED_PRIVATE_IP

The connection uses the existing Linux OpenSSH service reached through Tailscale networking.

Tailscale connectivity does not replace SSH key authentication or Linux account permissions.

## Firewall Integration

Tailscale was integrated with the existing host firewall hardening on HUB and RN.

The nodes were configured with Tailscale netfilter management disabled:

    netfilter-mode=off

This preserves manual responsibility for firewall access rules through iptables and netfilter-persistent.

Remote access requires the appropriate host firewall exceptions.

Firewall changes must preserve both remote management and the existing LAN security restrictions.

## ROG GitHub Workstation

The ROG also has:

- Git for Windows
- GitHub CLI
- Authenticated access to tedkphipps-ops
- Local clone of tedkphipps-ops/infra-ops

Git author identity:

    Ted Phipps <REDACTED_EMAIL>

Local repository:

    C:\Users\local_user\infra-ops

This provides a second workstation for infrastructure documentation and Git operations.

## Validation Status

- Tailscale connectivity between ROG, HUB, and RN: verified
- Remote OpenSSH access to HUB: verified
- Remote OpenSSH access to RN: verified
- Remote RN maintenance and service checks: verified October 31, 2026
- GitHub repository cloned on ROG: verified October 31, 2026

## Operational Notes

Lenovo WSL remains the established Ansible control environment.

The ROG provides an additional remote administration and documentation workstation.

Do not assume the ROG has an identical Ansible environment to Lenovo.

After changing Tailscale, SSH, or firewall settings, validate remote access before ending the maintenance session.
