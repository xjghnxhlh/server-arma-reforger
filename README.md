# Tutoriel complet : Créer son propre serveur **Arma Reforger** sur un VPS

## 1. Prérequis

### 1.1 Configuration VPS recommandée

* VPS **Ubuntu 22.04** (obligatoire pour la stabilité)
* 4 cœurs CPU minimum (6 à 8 recommandés)
* 8 Go de RAM minimum (16 Go conseillé)
* 60 Go d’espace disque
* Accès **root** ou sudo

### 1.2 Côté joueur

* Arma Reforger installé via Steam
* Version identique serveur / client

---

## 2. Connexion au VPS

Connexion SSH depuis ton PC :

```bash
ssh root@IP_DU_VPS
```

Mise à jour du système :

```bash
apt update && apt upgrade -y
```

---

## 3. Installation des dépendances

```bash
apt install -y lib32gcc-s1 lib32stdc++6 wget screen nano curl
```

---

## 4. Installation de SteamCMD

### 4.1 Création d’un utilisateur dédié

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

---

## 5. Installation du serveur Arma Reforger

Dans SteamCMD :

```bash
login anonymous
force_install_dir /home/steam/reforger
app_update 1874880 validate
quit
```

Le serveur est maintenant installé.

---

## 6. Structure du serveur

Dossier principal :

```text
/home/steam/reforger
```

Exécutable serveur :

```text
cd /home/steam/reforger
./ArmaReforgerServer -config server.json
```

---

## 7. Configuration du serveur

### 7.1 Création du fichier server.json

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

  "adminPassword": "admin123",
  "logging": {
    "logLevel": "NORMAL"
  }
}
```

---

## 8. Ouverture des ports

Ports requis :

* **2001 UDP** (jeu)
* **17777 UDP** (backend)

Avec UFW :

```bash
ufw allow 2001/udp
ufw allow 17777/udp
ufw reload
```

---

## 9. Script de lancement

Créer le script :

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

---

## 10. Lancer le serveur

Avec Screen :

```bash
screen -S reforger
./start.sh
```

Pour quitter sans arrêter : **CTRL + A puis D**

---

## 11. Connexion au serveur

Dans Arma Reforger :

1. Multijoueur
2. Serveurs communautaires
3. Rechercher le nom du serveur
4. Se connecter

---

## 12. Administration en jeu

* Ouvrir le chat
* Entrer :

```text
#login admin123
```

---

## 13. Installation de mods

### 13.1 Mods Workshop

* Les mods sont téléchargés automatiquement
* Ajouter les IDs dans server.json

Exemple :

```json
"mods": [
  { "modId": "596F55CDA6F4F3C1" }
]
```

---

## 14. Sécurité et optimisation

* Mot de passe admin fort
* Limiter les mods inutiles
* Redémarrage quotidien conseillé
* Sauvegarde du dossier reforger

---

## 15. Dépannage rapide

* Serveur invisible → ports fermés ou IP incorrecte
* Crash → mod incompatible
* Lag → RAM insuffisante

---

## 16. Conclusion

Ton **serveur Arma Reforger sur VPS** est maintenant opérationnel.

Il est prêt pour :

* Conflit
* Coopération
* RP moderne

---

https://www.youtube.com/watch?v=PJcGhNGB3pQ
