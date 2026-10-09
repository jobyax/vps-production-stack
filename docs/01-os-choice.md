# 01 — Choix du système d'exploitation

Pour un VPS en production, deux distributions de référence : **Debian** et **Ubuntu**.

## Comparatif

| Critère          | Debian                      | Ubuntu LTS                     |
| ---------------- | --------------------------- | ------------------------------ |
| Stabilité        | Très élevée                 | Élevée                         |
| Cycle de version | ~2 ans                      | 2 ans (LTS)                    |
| Logiciels        | Récents mais stables        | Plus récents                   |
| Ressources       | Légère                      | Légèrement plus lourde         |
| Documentation    | Exhaustive                  | Très accessible                |
| Support tiers    | Bon                         | Excellent (Docker, K8s, Cloud) |
| Recommandé pour  | Serveurs sobres et durables | Écosystème moderne et cloud    |

## Recommandation

- **Débutant / stack moderne (Docker, Web, CI)** → **Ubuntu LTS**
- **Serveur minimal, longue durée, maîtrise totale** → **Debian stable**

Dans ce guide, on part du principe que le serveur tourne sous **Debian** (les commandes sont identiques sous Ubuntu).

## Vérifier son système

```bash
cat /etc/os-release
```

Exemple de sortie :

```text
PRETTY_NAME="Debian GNU/Linux 13 (trixie)"
NAME="Debian GNU/Linux"
VERSION_ID="13"
VERSION="13 (trixie)"
VERSION_CODENAME=trixie
DEBIAN_VERSION_FULL=13.7
ID=debian
HOME_URL="https://www.debian.org/"
SUPPORT_URL="https://www.debian.org/support"
BUG_REPORT_URL="https://bugs.debian.org/"
```

## Prochaine étape

→ [02 — Création d'un utilisateur sudo](02-sudo-user.md)
