# Production CI/CD Deployment Pipeline for Ubuntu / Apache / MariaDB

**Language versions:** [Cesky](README_CZ.md) | [Deutsch](README_DE.md)

**Scope:** Automated deployment pipeline using GitHub Actions to deploy web applications to an Ubuntu server running Apache and MariaDB.
**Author:** Radim Kulaviak (isol.cz)

## Features
- Automated code deployment on `main` branch push.
- Secure SSH deployment using encrypted GitHub Secrets.
- Automatic Apache web server reload after successful code sync.
