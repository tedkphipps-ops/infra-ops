# SSH Access Workflow

This document records the Lenovo WSL SSH workflow for accessing homelab nodes and running Ansible.

## Local Control Host

| System | User | Purpose |
|---|---|---|
| Lenovo WSL | `local_user` | Local control host for SSH and Ansible |

## Remote Nodes

| Node | Hostname | IP | SSH User |
|---|---|---|---|
| HUB | `infra-hub` | `REDACTED_PRIVATE_IP` | `your_ssh_username` |
| RN | `redundant-net` | `REDACTED_PRIVATE_IP` | `your_ssh_username` |

## SSH Key

The Lenovo WSL control host uses a dedicated homelab SSH key.

| Item | Path |
|---|---|
| Private key | `~/.ssh/id_ed25519_homelab` |
| Public key | `~/.ssh/id_ed25519_homelab.pub` |

The public key is installed for `your_ssh_username` on both HUB and RN.

The private key is passphrase-protected. The passphrase is not stored in this repository.

## SSH Short Names

The Lenovo WSL SSH config includes short names for the homelab nodes.

| Short Name | Target |
|---|---|
| `hub` | `your_ssh_username@REDACTED_PRIVATE_IP` |
| `infra-hub` | `your_ssh_username@REDACTED_PRIVATE_IP` |
| `rn` | `your_ssh_username@REDACTED_PRIVATE_IP` |
| `redundant-net` | `your_ssh_username@REDACTED_PRIVATE_IP` |

Example checks:

| Command | Expected Result |
|---|---|
| `ssh hub 'hostname'` | `infra-hub` |
| `ssh rn 'hostname'` | `redundant-net` |

## Unlock Helper

A local helper exists on Lenovo WSL:

| Helper | Location |
|---|---|
| `homelab-unlock` | `/home/local_user/.local/bin/homelab-unlock` |

The helper loads the homelab SSH key into the SSH agent for one hour.

## Normal Startup Workflow

When opening a new Lenovo WSL terminal:

1. Run `cd /home/local_user/infra-ops`
2. Run `homelab-unlock`
3. Run `ansible homelab -i ansible/inventory/homelab.ini -m ping`

After entering the SSH key passphrase once, normal SSH and Ansible commands should work without the remote SSH password for about one hour.

## Ansible Usage

With the key loaded, Ansible can run without `-k`.

Validated command:

`ansible homelab -i ansible/inventory/homelab.ini -m ping`

Expected result:

| Node | Result |
|---|---|
| `infra-hub` | `SUCCESS` |
| `redundant-net` | `SUCCESS` |

## Password Notes

There are two separate password concepts.

| Password Type | Purpose |
|---|---|
| SSH key passphrase | Unlocks the Lenovo WSL private SSH key |
| Remote SSH password | Logs into `your_ssh_username` directly when key auth is not available |

The SSH key passphrase is not stored in this repository.

## Sudo Notes

This SSH workflow reduces repeated SSH login prompts.

It does not remove sudo prompts on HUB or RN. Administrative commands may still require the `your_ssh_username` sudo password.

## Result

SSH key authentication is configured from Lenovo WSL to both homelab nodes.

The validated workflow supports:

* `ssh hub`
* `ssh rn`
* Ansible without `-k` after running `homelab-unlock`
