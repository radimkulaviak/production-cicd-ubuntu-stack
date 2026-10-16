# Produkcni CI/CD Pipeline pro Ubuntu / Apache / MariaDB

**Jazykove verze:** [English](README.md) | [Deutsch](README_DE.md)

**Rozsah:** Automatizovana nasazovaci pipeline vyuzivajici GitHub Actions pro nasazovani webovych aplikaci na Ubuntu server s Apache a MariaDB.
**Autor:** Radim Kulaviak (isol.cz)

## Funkce
- Automaticke nasazeni kodu pri pushnuti do vetve `main`.
- Bezpecne SSH nasazeni pomoci sifrovanych GitHub Secrets.
- Automaticky reload weboveho serveru Apache po uspesne synchronizaci kodu.
