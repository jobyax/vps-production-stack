# 04 — Installation de Docker Engine

L'objectif est d'installer Docker Engine, `containerd`, `docker-buildx-plugin` et `docker-compose-plugin` depuis le dépôt officiel de Docker.

## 1. Désinstaller d'anciennes versions

Certaines distributions fournissent des paquets Docker non-officiels. On les supprime pour éviter tout conflit.

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc docker-buildx podman-docker containerd runc 2>/dev/null | cut -f1)
```

> [!NOTE]
> `apt` peut indiquer qu'aucun de ces paquets n'est installé, c'est tout à fait normal.

## 2. Configurer le dépôt officiel de Docker

### Pour Debian

```bash
# Mettre à jour les paquets et installer les dépendances
sudo apt update
sudo apt install ca-certificates curl

# Créer le dossier pour les clés GPG
sudo install -m 0755 -d /etc/apt/keyrings

# Ajouter la clé GPG officielle de Docker
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Ajouter le dépôt APT officiel
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

# Mettre à jour à nouveau
sudo apt update
```

### Pour Ubuntu

```bash
# Mettre à jour les paquets et installer les dépendances
sudo apt update
sudo apt install ca-certificates curl

# Créer le dossier pour les clés GPG
sudo install -m 0755 -d /etc/apt/keyrings

# Ajouter la clé GPG officielle de Docker
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Ajouter le dépôt APT officiel
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

# Mettre à jour à nouveau
sudo apt update
```

## 3. Installer Docker Engine

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

| Paquet                  | Rôle                                 |
| ----------------------- | ------------------------------------ |
| `docker-ce`             | Moteur Docker (daemon)               |
| `docker-ce-cli`         | CLI `docker`                         |
| `containerd.io`         | Runtime de conteneurs                |
| `docker-buildx-plugin`  | Build multi-plateforme               |
| `docker-compose-plugin` | Plugin Compose V2 (`docker compose`) |

## 4. Démarrer et activer Docker

```bash
sudo systemctl enable --now docker
```

## 5. Utiliser Docker sans `sudo`

```bash
sudo usermod -aG docker $USER
newgrp docker
```

## 6. Tester l'installation

```bash
docker run hello-world
```

Vérifie également Docker Compose :

```bash
docker compose version
```
