# GitHub Actions: SSH Deployment to an Ubuntu / Apache Server

**Language versions:** [Česky](README_CZ.md) | [Deutsch](README_DE.md)

**Scope:** example of two GitHub Actions workflows for a web application on an Ubuntu server with Apache.
**Author:** Radim Kulaviak (isol.cz)

## Workflows
- **`ci.yml`** runs on every push and pull request: `yamllint` for the workflow files and `shellcheck` for shell scripts.
- **`deploy.yml`** is started **manually** (`workflow_dispatch`). It connects over SSH, runs `git pull --ff-only` in the application directory and reloads Apache.

## Required repository secrets
| Secret | Content |
|---|---|
| `DEPLOY_SSH_PRIVATE_KEY` | private key of a dedicated deploy user |
| `DEPLOY_KNOWN_HOSTS` | output of `ssh-keyscan -t ed25519 your.server` (verify the fingerprint yourself) |
| `DEPLOY_HOST` | server hostname or IP |
| `DEPLOY_USER` | deploy user on the server |

The deploy user needs write access to the application directory and `sudo` only for `systemctl reload apache2`.

*Scope: deployment example for a single server, no database handling.*
