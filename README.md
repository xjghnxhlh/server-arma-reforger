# 🪖 Créer son propre serveur Arma Reforger sur un VPS

![Platform](https://img.shields.io/badge/platform-Linux%20VPS-blue.svg)
![Game](https://img.shields.io/badge/game-Arma%20Reforger-success.svg)

Tutoriel complet pour installer, configurer et lancer un serveur
**Arma Reforger** dédié sur un VPS Linux, de l'installation de SteamCMD jusqu'à
la connexion en jeu.

## 📋 Sommaire

1. [Prérequis](#1-prérequis)
2. [Connexion au VPS](#2-connexion-au-vps)
3. [Installation des dépendances](#3-installation-des-dépendances)
4. [Installation de SteamCMD](#4-installation-de-steamcmd)
5. [Installation du serveur Arma Reforger](#5-installation-du-serveur-arma-reforger)
6. [Structure du serveur](#6-structure-du-serveur)
7. [Configuration du serveur](#7-configuration-du-serveur)
8. [Ouverture des ports](#8-ouverture-des-ports)
9. [Script de lancement](#9-script-de-lancement)
10. [Lancer le serveur](#10-lancer-le-serveur)
11. [Connexion au serveur](#11-connexion-au-serveur)
12. [Administration en jeu](#12-administration-en-jeu)
13. [Installation de mods](#13-installation-de-mods)
14. [Sécurité et optimisation](#14-sécurité-et-optimisation)
15. [Dépannage rapide](#15-dépannage-rapide)
16. [Conclusion](#16-conclusion)

---

## 1. Prérequis

### 1.1 Configuration VPS recommandée

- VPS **Ubuntu 22.04** (recommandé pour la stabilité)
- 4 cœurs CPU minimum (6 à 8 conseillés)
- 8 Go de RAM minimum (16 Go conseillés)
- 60 Go d'espace disque
- Accès **root** ou sudo

### 1.2 Côté joueur

- Arma Reforger installé via Steam
- Version identique entre serveur et client

## 2. Connexion au VPS

```bash
ssh root@IP_DU_VPS
```

Mise à jour du système :

```bash
apt update && apt upgrade -y
```

## 3. Installation des dépendances

```bash
apt install -y lib32gcc-s1 lib32stdc++6 wget screen nano curl
```

## 4. Installation de SteamCMD

### 4.1 Création d'un utilisateur dédié

```bash
adduser steam
su - steam
```

### 4.2 Téléchargement de SteamCMD

```bash
mkdir ~/steamcmd && cd ~/steamcmd
wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
tar -xvzf steamcmd_linux.tar.gz
```

Lancement initial :

```bash
./steamcmd.sh
```

## 5. Installation du serveur Arma Reforger

Dans SteamCMD :

```text
login anonymous
force_install_dir /home/steam/reforger
app_update 1874900 validate
quit
```

Le serveur est maintenant installé.

## 6. Structure du serveur

Dossier principal :

```text
/home/steam/reforger
```

Exécutable serveur :

```bash
cd /home/steam/reforger
./ArmaReforgerServer -config server.json
```

## 7. Configuration du serveur

### 7.1 Création du fichier `server.json`

```bash
nano ~/reforger/server.json
```

Configuration de base :

```json
{
  "bindAddress": "0.0.0.0",
  "bindPort": 2001,
  "publicAddress": "IP_DU_VPS",
  "publicPort": 2001,

  "maxPlayers": 48,
  "game": {
    "name": "Mon Serveur Arma Reforger",
    "scenarioId": "{ECC61978EDCC2B5A}",
    "password": "",
    "visible": true
  },

  "adminPassword": "CHANGE_MOI",
  "logging": {
    "logLevel": "NORMAL"
  }
}
```

> ⚠️ Remplace `adminPassword` par un mot de passe fort et unique avant de
> mettre le serveur en ligne — ne garde jamais une valeur par défaut.

## 8. Ouverture des ports

Ports requis :

- **2001/UDP** (jeu)
- **17777/UDP** (backend)

Avec UFW :

```bash
ufw allow 2001/udp
ufw allow 17777/udp
ufw reload
```

## 9. Script de lancement

```bash
nano ~/reforger/start.sh
```

```bash
#!/bin/bash
./ArmaReforgerServer \
  -config server.json \
  -maxFPS 60
```

Rendre exécutable :

```bash
chmod +x start.sh
```

## 10. Lancer le serveur

Avec `screen` :

```bash
screen -S reforger
./start.sh
```

Pour quitter sans arrêter le serveur : **Ctrl+A** puis **D**.

## 11. Connexion au serveur

Dans Arma Reforger :

1. Multijoueur
2. Serveurs communautaires
3. Rechercher le nom du serveur
4. Se connecter

## 12. Administration en jeu

Ouvrir le chat et entrer :

```text
#login CHANGE_MOI
```

(remplace par le mot de passe défini dans `adminPassword`)

## 13. Installation de mods

### 13.1 Mods Workshop

Les mods sont téléchargés automatiquement ; ajoute simplement leurs IDs dans
`server.json` :

```json
"mods": [
  { "modId": "596F55CDA6F4F3C1" }
]
```

## 14. Sécurité et optimisation

- Mot de passe admin fort et unique
- Limiter le nombre de mods actifs
- Redémarrage quotidien conseillé (via `cron` + `screen`)
- Sauvegarder régulièrement le dossier `reforger`

## 15. Dépannage rapide

| Symptôme           | Cause probable              |
| ------------------ | ---------------------------- |
| Serveur invisible   | Ports fermés ou IP incorrecte |
| Crash au démarrage  | Mod incompatible             |
| Lag / freeze        | RAM insuffisante             |

## 16. Conclusion

Ton **serveur Arma Reforger sur VPS** est maintenant opérationnel, prêt pour
du Conflict, de la Coopération ou du RP moderne.

📺 **Vidéo de démonstration :** [youtube.com/watch?v=PJcGhNGB3pQ](https://www.youtube.com/watch?v=PJcGhNGB3pQ)
