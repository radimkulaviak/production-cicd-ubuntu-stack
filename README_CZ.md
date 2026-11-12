# GitHub Actions: nasazení přes SSH na server Ubuntu / Apache

**Jazykové verze:** [English](README.md) | [Deutsch](README_DE.md)

**Rozsah:** příklad dvou workflow GitHub Actions pro webovou aplikaci na serveru Ubuntu s Apache.
**Autor:** Radim Kulaviak (isol.cz)

## Workflow
- **`ci.yml`** běží při každém pushi a pull requestu: `yamllint` pro soubory workflow a `shellcheck` pro shellové skripty.
- **`deploy.yml`** se spouští **ručně** (`workflow_dispatch`). Připojí se přes SSH, v adresáři aplikace provede `git pull --ff-only` a znovu načte Apache.

## Potřebné secrets v repozitáři
| Secret | Obsah |
|---|---|
| `DEPLOY_SSH_PRIVATE_KEY` | privátní klíč vyhrazeného deploy uživatele |
| `DEPLOY_KNOWN_HOSTS` | výstup `ssh-keyscan -t ed25519 tvuj.server` (otisk si ověř sám) |
| `DEPLOY_HOST` | hostname nebo IP serveru |
| `DEPLOY_USER` | deploy uživatel na serveru |

Deploy uživatel potřebuje zápis do adresáře aplikace a `sudo` jen pro `systemctl reload apache2`.

*Rozsah: příklad nasazení na jeden server, bez práce s databází.*
