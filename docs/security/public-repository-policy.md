# Public Repository Security Policy

## Purpose

The infra-ops repository documents homelab architecture,
automation, maintenance, and troubleshooting as a public
technical portfolio.

Public documentation must demonstrate technical skills without
exposing private infrastructure, credentials, or personal data.

## Information That Must Remain Private

Never commit or publish:

- Real internal or external IP addresses, subnets, or MAC addresses.
- Tailscale addresses, tailnet identifiers, credentials, and private connection details.
- Real SSH usernames, private keys, access configuration, or credentials.
- Passwords, API tokens, authentication secrets, or recovery codes.
- Household members' names, private email addresses, and personally identifying device labels.
- Live DNS records, DHCP mappings, and sensitive network access rules.
- Operational inventories containing real connection details.
- Screenshots, diagrams, or logs exposing sensitive information.
- Private backup files or environment configuration.

## Public Documentation Standards

Use:

- Documentation IP ranges such as 192.0.2.0/24.
- Generic usernames such as your_ssh_username.
- Descriptive, non-personal hostnames such as infra-hub and redundant-net.
- Real equipment manufacturers, models, and useful hardware specifications.
- Anonymous labels for devices named after household members.
- Detailed, sanitized examples of Ansible, Docker, networking, and monitoring.
- Accurate architecture diagrams without sensitive connection information.
- Aligned Markdown tables, consistent headings, and readable code blocks.

Preserve service configurations, validated troubleshooting results,
and technical accomplishments wherever they can be documented safely.
Do not unnecessarily replace meaningful technical details with generic descriptions.

## Ansible Inventory Protection

The operational inventory is:

ansible/inventory/homelab.ini

It must remain local and excluded from Git tracking.

The public example is:

ansible/inventory/homelab.example.ini

The example must contain only documentation addresses and
generic account information.

Changes to inventory handling must preserve functioning
local Ansible operations.

## Git Security Requirements

- Use .gitignore to exclude private configuration and secrets.
- Review git status and git diff before committing.
- Inspect new files for credentials and identifying information.
- Review images and screenshots before publishing.
- Never assume .gitignore protects previously tracked files.
- Do not commit live credentials, even temporarily.
- Do not push changes until security and functionality checks pass.

## Production Change Requirements

Production configuration changes must be validated before
their sanitized documentation is committed.

Public documentation must not become the storage location
for private operational secrets or connection information.

## Previously Published Information

Removing sensitive information from current files does not
remove it from Git history, forks, or external caches.

Previously exposed credentials must be rotated or revoked.

Historical exposure must be reviewed separately before
considering repository history cleanup.

## Review Standard

Before every push, verify that:

1. No credentials, private addressing, or unintended personal identifiers are included.
2. Public configuration examples contain no operational secrets.
3. Existing homelab functionality remains intact.
4. Documentation reflects validated production behavior.
5. The final Git diff contains only intended changes.
6. Markdown tables and documentation formatting are consistent.
7. Valuable technical details have not been unnecessarily removed.
