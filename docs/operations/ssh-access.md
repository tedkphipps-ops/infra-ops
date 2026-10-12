# SSH Access Workflow

This document records the Lenovo WSL SSH workflow for accessing homelab nodes and running Ansible.

## Local Control Host

| System | User | Purpose |
|---|---|---|
| Lenovo WSL | `tedkp` | Local control host for SSH and Ansible |

## Remote Nodes

| Node | Hostname | IP | SSH User |
|---|---|---|---|
| HUB | `infra-hub` | `192.168.1.225` | `ted_phipps` |
| RN | `redundant-net` | `192.168.1.237` | `ted_phipps` |

## SSH Key

The Lenovo WSL control host uses a dedicated homelab SSH key.

| Item | Path |
|---|---|
| Private key | `~/.ssh/id_ed25519_homelab` |
| Public key | `~/.ssh/id_ed25519_homelab.pub` |

The public key is installed for `ted_phipps` on both HUB and RN.

The private key is passphrase-protected. The passphrase is not stored in this repository.

## SSH Short Names

The Lenovo WSL SSH config includes short names for the homelab nodes.

| Short Name | Target |
|---|---|
| `hub` | `ted_phipps@192.168.1.225` |
| `infra-hub` | `ted_phipps@192.168.1.225` |
| `rn` | `ted_phipps@192.168.1.237` |
| `redundant-net` | `ted_phipps@192.168.1.237` |

Example checks:

| Command | Expected Result |
|---|---|
| `ssh hub 'hostname'` | `infra-hub` |
| `ssh rn 'hostname'` | `redundant-net` |

## Unlock Helper

A local helper exists on Lenovo WSL:

| Helper | Location |
|---|---|
| `homelab-unlock` | `/home/tedkp/.local/bin/homelab-unlock` |

The helper loads the homelab SSH key into the SSH agent for one hour.

## Normal Startup Workflow

When opening a new Lenovo WSL terminal:

1. Run `cd /home/tedkp/infra-ops`
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
| Remote SSH password | Logs into `ted_phipps` directly when key auth is not available |

The SSH key passphrase is not stored in this repository.

## Sudo Notes

This SSH workflow reduces repeated SSH login prompts.

It does not remove sudo prompts on HUB or RN. Administrative commands may still require the `ted_phipps` sudo password.

## Result

SSH key authentication is configured from Lenovo WSL to both homelab nodes.

The validated workflow supports:

* `ssh hub`
* `ssh rn`
* Ansible without `-k` after running `homelab-unlock`
