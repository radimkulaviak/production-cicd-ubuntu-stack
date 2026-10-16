# Produktions-CI/CD-Pipeline fuer Ubuntu / Apache / MariaDB

**Sprachversionen:** [English](README.md) | [Cesky](README_CZ.md)

**Umfang:** Automatisierte Deployment-Pipeline mit GitHub Actions zur Bereitstellung von Webanwendungen auf einem Ubuntu-Server mit Apache und MariaDB.
**Autor:** Radim Kulaviak (isol.cz)

## Funktionen
- Automatisiertes Code-Deployment beim Push in den `main`-Branch.
- Sicheres SSH-Deployment ueber verschluesselte GitHub Secrets.
- Automatischer Neustart des Apache-Webservers nach erfolgreicher Codesynchronisation.
