# GitHub Actions: SSH-Deployment auf einen Ubuntu-/Apache-Server

**Sprachversionen:** [English](README.md) | [Česky](README_CZ.md)

**Umfang:** Beispiel für zwei GitHub-Actions-Workflows für eine Webanwendung auf einem Ubuntu-Server mit Apache.
**Autor:** Radim Kulaviak (isol.cz)

## Workflows
- **`ci.yml`** läuft bei jedem Push und Pull Request: `yamllint` für die Workflow-Dateien und `shellcheck` für Shell-Skripte.
- **`deploy.yml`** wird **manuell** gestartet (`workflow_dispatch`). Es verbindet sich per SSH, führt im Anwendungsverzeichnis `git pull --ff-only` aus und lädt Apache neu.

## Benötigte Repository-Secrets
| Secret | Inhalt |
|---|---|
| `DEPLOY_SSH_PRIVATE_KEY` | privater Schlüssel eines dedizierten Deploy-Benutzers |
| `DEPLOY_KNOWN_HOSTS` | Ausgabe von `ssh-keyscan -t ed25519 dein.server` (Fingerprint selbst prüfen) |
| `DEPLOY_HOST` | Hostname oder IP des Servers |
| `DEPLOY_USER` | Deploy-Benutzer auf dem Server |

Der Deploy-Benutzer braucht Schreibrechte im Anwendungsverzeichnis und `sudo` nur für `systemctl reload apache2`.

*Umfang: Deployment-Beispiel für einen einzelnen Server, ohne Datenbankbehandlung.*
