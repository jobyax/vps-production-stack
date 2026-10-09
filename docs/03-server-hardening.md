# 03 — Durcissement du serveur

Durcir l'accès SSH : clé d'authentification, suppression du mot de passe
et désactivation du login root. Configurer ensuite le pare-feu UFW.

## Étapes

### 1. Générer une clé SSH (côté client)

```bash
ssh-keygen -t ed25519 -C "toto@vps"
```

Choisis un chemin (`~/.ssh/id_ed25519`) et une phrase de passe.

### 2. Copier la clé publique sur le serveur

```bash
ssh-copy-id toto@IP_DU_VPS
```

Teste la connexion **sans mot de passe** avant de continuer :

```bash
ssh toto@IP_DU_VPS
```

### 3. Durcir la configuration SSH

```bash
sudo nano /etc/ssh/sshd_config
```

Paramètres à définir :

```text
PermitRootLogin no           # interdit la connexion directe en root
PasswordAuthentication no    # refuse les mots de passe, clé SSH uniquement
PubkeyAuthentication yes     # autorise l'authentification par clé publique
AllowUsers toto              # liste blanche : seuls ces comptes se connectent
MaxAuthTries 3               # coupe la connexion après 3 tentatives échouées
LoginGraceTime 20            # 20 s pour s'authentifier, sinon déconnexion
X11Forwarding no             # désactive le transfert graphique X11 (inutile)
```

### 4. Appliquer la configuration

```bash
sudo sshd -t          # tester la config
sudo systemctl restart ssh
```

Garde une seconde session ouverte en secours jusqu'à validation.

### 5. Configurer le pare-feu UFW

```bash
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
```

- **Trafic entrant** : bloqué par défaut
- **Trafic sortant** : autorisé par défaut
- **Ports explicitement autorisés** : 22 (SSH), 80 (HTTP), 443 (HTTPS)

Vérifier les règles :

```bash
sudo ufw status verbose
```

## Optionnel — Connexion SSH simplifiée

Se connecter en précisant la clé :

```bash
ssh -i ~/.ssh/id_ed25519 toto@IP_DU_VPS
```

Créer un alias pour se connecter avec une simple commande `ssh name-server` :

```bash
nano ~/.ssh/config
```

```text
Host name-server
    HostName IP_DU_VPS
    User toto
    Port 22
    IdentityFile ~/.ssh/id_ed25519
```

Connexion :

```bash
ssh name-server
```

## Récapitulatif

| Mesure                      | Effet                              |
| --------------------------- | ---------------------------------- |
| Clé SSH (ed25519)           | Authentification sans mot de passe |
| `PasswordAuthentication no` | Bloque le brute-force              |
| `PermitRootLogin no`        | Login root interdit via SSH        |
| `AllowUsers toto`           | Limite les comptes autorisés       |
| `MaxAuthTries 3`            | Limite les tentatives              |
| UFW : entrant bloqué        | Refuse tout par défaut             |
| UFW : ports 22, 80, 443     | Autorise SSH, HTTP, HTTPS          |

## Note

Une fois `PasswordAuthentication no` actif, seule la clé SSH permet la connexion.
Ne ferme jamais ta session actuelle avant d'avoir validé la nouvelle.
