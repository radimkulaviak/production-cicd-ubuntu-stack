# CI/CD nasazení přes GitHub Actions a SSH (Ubuntu / Apache / MariaDB)

**Jazykové verze:** [English](README.md) | [Deutsch](README_DE.md)

**Rozsah:** Workflow GitHub Actions, které nasazuje webovou aplikaci na Ubuntu server s Apache. Funguje s libovolným serverem dostupným přes SSH (on-premise, VPS, cloudová VM).
**Autor:** Radim Kulaviak ([isol.cz](https://www.isol.cz))

**Stav:** ukázkový workflow, zatím nespuštěný proti produkčnímu serveru. Po otestování tento řádek upravte.

## Jak to funguje
- Ve výchozím stavu se spouští ručně (`workflow_dispatch`); trigger `push` odkomentujte až po nastavení secrets. Souběžná nasazení se řadí za sebou.
- Kód se nahraje přes `rsync` do `releases/<čas>-<sha>`; `current` je symlink přepínaný atomicky.
- Po přepnutí se provede reload Apache a **health check**. Při selhání se automaticky vrátí předchozí release.
- Pro ruční rollback se ponechává pět posledních releasů.
- Host key je připnutý přes secret (`DEPLOY_KNOWN_HOSTS`); žádné `StrictHostKeyChecking=no`.

## Potřebné secrets
| Secret | Obsah |
|---|---|
| `DEPLOY_SSH_PRIVATE_KEY` | privátní klíč vyhrazeného deploy uživatele |
| `DEPLOY_KNOWN_HOSTS` | výstup `ssh-keyscan -t ed25519 vas.host` (otisk ověřte ručně) |
| `DEPLOY_HOST` / `DEPLOY_USER` | cílový server a uživatel |

## Příprava serveru
```bash
# deploy uživatel, adresář releasů, DocumentRoot Apache -> /var/www/my-app/current
sudo adduser --disabled-password --gecos "" deploy
sudo install -d -o deploy -g deploy /var/www/my-app/releases
echo 'deploy ALL=(root) NOPASSWD: /usr/bin/systemctl reload apache2' | sudo tee /etc/sudoers.d/deploy
sudo chmod 440 /etc/sudoers.d/deploy
```
Upravte `APP_DIR` a `HEALTH_URL` v `.github/workflows/deploy.yml`. Migrace databáze záměrně nejsou součástí příkladu — vyžadují vlastní, zpětně kompatibilní proces.
