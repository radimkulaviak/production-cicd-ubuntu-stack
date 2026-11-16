# CI/CD Deployment via GitHub Actions and SSH (Ubuntu / Apache / MariaDB)

**Language versions:** [Česky](README_CZ.md) | [Deutsch](README_DE.md)

**Scope:** GitHub Actions workflow that deploys a web application to an Ubuntu server running Apache. Works with any SSH-reachable server (on-premises, VPS, cloud VM).
**Author:** Radim Kulaviak ([isol.cz](https://www.isol.cz))

**Status:** example workflow, not yet run against a production server. Update this line after testing.

## How it works
- Manual trigger by default (`workflow_dispatch`); enable the commented `push` trigger in the workflow once secrets are set. Concurrent deployments are serialised.
- Code is uploaded with `rsync` into `releases/<timestamp>-<sha>`; `current` is a symlink switched atomically.
- After the switch, Apache is reloaded and a **health check** runs. If it fails, the previous release is restored automatically.
- The five most recent releases are kept for manual rollback.
- Host key is pinned via a secret (`DEPLOY_KNOWN_HOSTS`); no `StrictHostKeyChecking=no`.

## Required secrets
| Secret | Content |
|---|---|
| `DEPLOY_SSH_PRIVATE_KEY` | private key of a dedicated deploy user |
| `DEPLOY_KNOWN_HOSTS` | output of `ssh-keyscan -t ed25519 your.host` (verify the fingerprint manually) |
| `DEPLOY_HOST` / `DEPLOY_USER` | target host and user |

## Server preparation
```bash
# deploy user, release directory, Apache DocumentRoot -> /var/www/my-app/current
sudo adduser --disabled-password --gecos "" deploy
sudo install -d -o deploy -g deploy /var/www/my-app/releases
echo 'deploy ALL=(root) NOPASSWD: /usr/bin/systemctl reload apache2' | sudo tee /etc/sudoers.d/deploy
sudo chmod 440 /etc/sudoers.d/deploy
```
Edit `APP_DIR` and `HEALTH_URL` in `.github/workflows/deploy.yml`. Database migrations are intentionally not part of this example — they need their own, backwards-compatible process.
