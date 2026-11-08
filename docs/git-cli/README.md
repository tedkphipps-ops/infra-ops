# Git and GitHub Operations

## Overview

The `infra-ops` repository maintains version-controlled documentation of a two-node Linux homelab, including deployed services, configuration practices, monitoring, maintenance, and troubleshooting.

GitHub repository: `tedkphipps-ops/infra-ops`

Changes are documented after they have been implemented and validated. The repository serves as the official published record of verified infrastructure work.

## Administration Workstations

Two administration workstations use local copies of the same GitHub repository:

- **Lenovo Command Station:** Ubuntu WSL, Git, SSH, and Ansible administration.

- **ROG Mobile Command:** Windows PowerShell, Git, GitHub CLI, and SSH administration.

Both workstations synchronize their local Git repositories with the same remote `main` branch.

## Synchronization Workflow

Before editing repository files:

1. Check the local working tree with `git status -sb`.

2. Retrieve remote changes with `git fetch origin main`.

3. Update a clean local branch with `git merge --ff-only origin/main`.

4. Confirm the expected repository state.

Uncommitted changes on one workstation are not automatically available on the other. Finish and publish validated work before switching workstations when practical.

## Change Management

The established documentation workflow is:

1. Inspect the current infrastructure or repository state.

2. Implement and validate the required production changes.

3. Update the relevant documentation.

4. Review changes with `git diff`.

5. Check formatting with `git diff --check`.

6. Stage only the intended files using `git add`.

7. Review staged changes with `git diff --cached`.

8. Commit completed work with a descriptive message.

9. Push to the official GitHub repository.

10. Verify synchronization with `git status -sb`.

## Repository Security

Operational credentials, private addressing, SSH keys, and sensitive configuration are excluded from public documentation.

Private Ansible inventory is maintained locally and protected through Git ignore rules.

Public documentation uses sanitized examples and describes verified technical outcomes without exposing unnecessary operational details.

## Documentation Principles

- Record completed and validated infrastructure work.

- Preserve useful troubleshooting findings and recovery procedures.

- Keep documentation consistent with production changes.

- Use clear commit messages and review changes before publishing.

- Keep unfinished roadmap discussions separate from the public repository.

## Summary

The Git/GitHub workflow provides version control, change traceability, documentation consistency, and coordination between both administration workstations.

It supports the maintenance of a professional, evidence-based record of homelab infrastructure operations.
