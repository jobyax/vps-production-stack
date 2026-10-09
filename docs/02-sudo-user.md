# 02 — Création d'un utilisateur sudo

On ne travaille jamais directement en `root`. On crée un compte utilisateur
puis on lui accorde les droits d'administration via `sudo`.

## Étapes

### 1. (Optionnel) Changer le mot de passe root par défaut

Si `root` dispose d'un mot de passe par défaut (fourni par l'hébergeur), change-le immédiatement :

```bash
passwd root
```

Saisis le nouveau mot de passe deux fois. C'est un prérequis avant toute autre action.

### 2. Créer l'utilisateur

```bash
adduser toto
```

Crée le compte `toto`, son répertoire personnel `/home/toto` et demande un mot de passe.

### 3. Ajouter au groupe `sudo`

```bash
usermod -aG sudo toto
```

`-aG` = ajouter (`-a`) au groupe (`-G`) sans retirer les groupes existants.

### 4. Se connecter avec le nouvel utilisateur

```bash
su - toto
```

`su -` ouvre une session complète avec l'environnement de `toto`.

### 5. Vérifier les privilèges

```bash
sudo whoami
```

Doit afficher :

```text
root
```

## Récapitulatif

| Commande                | Rôle                         |
| ----------------------- | ---------------------------- |
| `passwd root`           | Changer le mot de passe root |
| `adduser toto`          | Créer l'utilisateur          |
| `usermod -aG sudo toto` | Accorder les droits sudo     |
| `su - toto`             | Basculer sur le compte       |
| `sudo whoami`           | Vérifier les droits root     |

## Note

Pour utiliser `sudo` sans mot de passe, voir l'étape suivante (configuration via `visudo`).
