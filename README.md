# Remote Control

Suite de contrôle à distance combinant Apache Guacamole (bureau à distance) et UpSnap (Wake-on-LAN).

## 📁 Fichiers de configuration

- **guacamole.yml** : Configuration Apache Guacamole
- **wakeonlan.yml** : Configuration UpSnap (Wake-on-LAN)
- **initdb.sql** : Script d'initialisation de la base de données
- **.volumes/** : Données persistantes
  - `guacamole_db/` : Base de données MariaDB
  - `guacd_drive/` : Partages de fichiers virtuels
  - `guacd_record/` : Enregistrements de sessions
  - `upsnap/` : Données UpSnap

## 🚀 Démarrage

### Guacamole
```bash
docker-compose -f guacamole.yml up -d
```

### Wake-on-LAN
```bash
docker-compose -f wakeonlan.yml up -d
```

## 🌐 Accès

- **Guacamole** : http://localhost:8081
  - Identifiants par défaut : `guacadmin` / `guacadmin` (à changer!)
- **UpSnap** : http://localhost (mode host)

## 📦 Services Guacamole

### guacamole_db (MariaDB)
- Base de données pour Guacamole
- Credentials configurés dans le compose

### guacd
- Daemon Guacamole pour les connexions distantes
- Support RDP, VNC, SSH

### guacamole
- Interface web pour gérer les connexions

## 🔐 Sécurité

**⚠️ IMPORTANT** : Changez les mots de passe par défaut dans `guacamole.yml` :
- `MYSQL_ROOT_PASSWORD`
- `MYSQL_PASSWORD`
- Identifiants Guacamole après la première connexion

## 💤 Wake-on-LAN (UpSnap)

UpSnap permet de réveiller des machines à distance via Wake-on-LAN avec une interface web moderne.

**Prérequis** :
- Wake-on-LAN activé dans le BIOS de la machine cible
- Mode `network_mode: host` pour l'envoi de paquets magiques

## ⚙️ Ports utilisés

| Port | Service | Description |
|------|---------|-------------|
| 8081 | Guacamole | Interface web |
| 4822 | Guacd | Daemon (interne) |
| 3306 | MariaDB | Base de données (interne) |
| Host | UpSnap | Interface Wake-on-LAN |

## 📋 Initialisation

Au premier démarrage de Guacamole, exécutez le script SQL :
```bash
docker exec -i guacamole_db mysql -uroot -p<password> guacamole_db < initdb.sql
```
