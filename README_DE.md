# CI/CD-Deployment per GitHub Actions und SSH (Ubuntu / Apache / MariaDB)

**Sprachversionen:** [English](README.md) | [Česky](README_CZ.md)

**Umfang:** GitHub-Actions-Workflow, der eine Webanwendung auf einen Ubuntu-Server mit Apache ausrollt. Funktioniert mit jedem per SSH erreichbaren Server (On-Premises, VPS, Cloud-VM).
**Autor:** Radim Kulaviak ([isol.cz](https://www.isol.cz))

**Status:** Beispiel-Workflow, bisher nicht gegen einen Produktionsserver ausgeführt. Nach dem Test diese Zeile anpassen.

## Funktionsweise
- Standardmäßig manuell auslösbar (`workflow_dispatch`); den `push`-Trigger erst nach Einrichtung der Secrets aktivieren. Parallele Deployments werden serialisiert.
- Der Code wird per `rsync` nach `releases/<Zeitstempel>-<sha>` hochgeladen; `current` ist ein atomar umgeschalteter Symlink.
- Nach dem Umschalten folgen Apache-Reload und **Health-Check**. Schlägt er fehl, wird automatisch das vorherige Release wiederhergestellt.
- Die fünf neuesten Releases bleiben für ein manuelles Rollback erhalten.
- Der Host-Key ist über ein Secret fixiert (`DEPLOY_KNOWN_HOSTS`); kein `StrictHostKeyChecking=no`.

## Benötigte Secrets
| Secret | Inhalt |
|---|---|
| `DEPLOY_SSH_PRIVATE_KEY` | privater Schlüssel eines dedizierten Deploy-Benutzers |
| `DEPLOY_KNOWN_HOSTS` | Ausgabe von `ssh-keyscan -t ed25519 ihr.host` (Fingerprint manuell prüfen) |
| `DEPLOY_HOST` / `DEPLOY_USER` | Zielserver und Benutzer |

## Serververbereitung
```bash
# Deploy-Benutzer, Release-Verzeichnis, Apache-DocumentRoot -> /var/www/my-app/current
sudo adduser --disabled-password --gecos "" deploy
sudo install -d -o deploy -g deploy /var/www/my-app/releases
echo 'deploy ALL=(root) NOPASSWD: /usr/bin/systemctl reload apache2' | sudo tee /etc/sudoers.d/deploy
sudo chmod 440 /etc/sudoers.d/deploy
```
`APP_DIR` und `HEALTH_URL` in `.github/workflows/deploy.yml` anpassen. Datenbankmigrationen sind bewusst nicht Teil dieses Beispiels — sie benötigen einen eigenen, abwärtskompatiblen Prozess.
