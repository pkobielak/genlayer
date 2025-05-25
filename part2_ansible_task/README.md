# Ansible Playbook: Multi-Repo Coordinator Setup

This playbook automates the setup of the `multi-repo-coordinator` repository on Ubuntu 22.04.

## Prerequisites

- Ubuntu 22.04 server with root or sudo access (remote)
- Python 3 installed with Ansible (local)

## Inventory

- `inventory/hosts.yml` defines the `coordinator` host (uses `localhost` by default).

## Usage

```bash
ansible-playbook -i inventory/hosts.yml setup_coordinator.yml
```

This will:

1. Install `git`and `yq`.
2. Create `/opt/multi-repo-coordinator`, initialize a Git repo with `main` branch and an empty commit.
3. Create `ci-bot` user with SSH keypair (public key in `~ci-bot/.ssh/authorized_keys`) and exported locally as `./ci-bot_github_key.pub`.
4. Render `manifest.yaml` with a sample changeset based on list of repos from variables.

## Post-Setup

- The playbook fetches the `ci-bot`'s public key to your local machine as `./ci-bot_github_key.pub`.
- Upload this public key to your Git hosting service as a deploy key or service account.
- Ensure CI pipeline uses the manifest and `ci-bot` credentials for repository operations.

## Assumptions

- There was no task to push the repository to any remote, but only to initialize it. I guess this is just an Ansible skills challenge, because full component of the solution would require a bigger scope if we are to go this route with setting all this with Ansible.
- Some of the steps left to make it more production-ready would be to:
  - Push repository to remote
  - Configure protected branch on `coordinator` repository
  - Upload ci-bot keys to Git hosting service (assuming ci-bot service account/GH App is set manually)
  - (Optional) configure initial CI pipeline
