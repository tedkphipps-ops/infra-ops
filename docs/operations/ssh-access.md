# Secure SSH Administration

## Overview

The homelab uses SSH for authenticated Linux administration
and Ansible automation.

Operational connection details are maintained privately.

## Authentication

SSH key authentication provides secure access to managed
Linux systems.

The implementation follows these principles:

- Use dedicated administrative SSH keys.
- Protect private keys with passphrases.
- Never commit authentication material to Git.
- Restrict access through appropriate permissions.
- Use SSH agents to manage authentication sessions.
- Maintain separate authentication and sudo authorization.

## Ansible Integration

Ansible communicates with managed Linux nodes through SSH.

The environment uses a private Ansible inventory containing
the connection settings required for automation.

The public inventory example is located at:

ansible/inventory/homelab.example.ini

## Validation

SSH key authentication and Ansible connectivity have
previously been validated against both managed nodes.

Historical validation does not guarantee current availability.

## Security Requirements

The public repository intentionally excludes:

- Operational IP addresses and host mappings
- Actual SSH usernames and key locations
- Private SSH configuration and helper scripts
- Authentication credentials and access procedures
- Sensitive administrative connection details

## Operational Documentation

Detailed access instructions are maintained privately.

See docs/security/public-repository-policy.md for
the repository's security requirements.
