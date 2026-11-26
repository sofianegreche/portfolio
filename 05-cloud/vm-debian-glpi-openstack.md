---
title: Création d'une VM Debian 13 pour GLPI sur OpenStack (Infomaniak)
tags: OpenStack, Infomaniak, Debian, GLPI, Hackathon
---

# Création d'une VM Debian 13 pour héberger GLPI

[TOC]

:::info
**Objectif :** créer sur le Public Cloud Infomaniak (OpenStack) une machine virtuelle **Debian 13** avec **4 vCPU, 8 Go de RAM et 50 Go de disque**, puis y installer **GLPI**.

**Projet OpenStack :** `PCP-YV8RZH7`, région `dc4-a`
**Interface :** Horizon, https://api.pub2.infomaniak.cloud/horizon/
:::

## Récapitulatif de la VM

| Élément | Valeur |
|---|---|
| Nom de l'instance | `vm-glpi` |
| Image | Debian 13 trixie |
| Gabarit (flavor) | `a4-ram8-disk50-perf1` (4 vCPU, 8 Go RAM, 50 Go disque) |
| Réseau | `ext-net1` (réseau public) |
| Adresse IP publique | `<IP_PUBLIQUE>` |
| Groupe de sécurité | `glpi-sg` (ports 22, 80, 443) |
| Paire de clés | `cle-glpi` |
| Utilisateur SSH | `debian` |

---

## Étape 1 : Se connecter à Horizon

1. Ouvrir https://api.pub2.infomaniak.cloud/horizon/
2. Saisir le nom d'utilisateur OpenStack (`PCU-…`) et le mot de passe fournis par la formatrice.
3. Cliquer sur **Se connecter**.

:::warning
Le lien `api.pub1…` redirige automatiquement vers `api.pub2…`. C'est normal.
:::

📸 *Capture : page de connexion Horizon (à faire toi-même)*
![](COLLER_CAPTURE_ICI)

---

## Étape 2 : Créer la paire de clés SSH

Les images Debian du cloud n'ont **pas de mot de passe** : la connexion se fait uniquement avec une clé SSH.

1. Menu **Projet → Compute → Paires de clés**
2. Cliquer sur **Créer une paire de clés**
3. Nom : `cle-glpi`, type : **Clé SSH**
4. Valider : le fichier **`cle-glpi.pem`** (clé privée) se télécharge automatiquement.

:::danger
La clé privée n'est téléchargée **qu'une seule fois**. Si elle est perdue, on ne peut plus se connecter à la VM. Ne jamais la partager (Teams, GitHub…).
:::

![01-paires-de-cles](https://hackmd.io/_uploads/rkVf5SuAGe.jpg)


---

## Étape 3 : Créer le groupe de sécurité

Le groupe de sécurité joue le rôle de **pare-feu** de la VM. Par défaut, tout le trafic entrant est bloqué.

1. Menu **Projet → Réseau → Groupes de sécurité**
2. **Créer un groupe de sécurité**. Nom : `glpi-sg`, description : `SSH, HTTP, HTTPS pour GLPI`
3. Sur `glpi-sg`, cliquer sur **Gérer les règles**, puis **Ajouter une règle**, trois fois :

| Règle | Direction | Port | Distant (CIDR) | Utilité |
|---|---|---|---|---|
| SSH | Entrée | 22 | 0.0.0.0/0 | Administration à distance |
| HTTP | Entrée | 80 | 0.0.0.0/0 | Accès web à GLPI |
| HTTPS | Entrée | 443 | 0.0.0.0/0 | Accès web sécurisé |

:::info
Les règles **Sortie** (egress) IPv4/IPv6 existent déjà par défaut : la VM peut accéder à Internet (mises à jour, téléchargement de GLPI).
:::

![02-groupe-securite-regles](https://hackmd.io/_uploads/ryzHcSdRzg.jpg)


---

## Étape 4 : Lancer l'instance

Menu **Projet → Compute → Instances → Lancer une instance**, puis remplir les onglets :

### 4.1 Détails
- **Nom de l'instance :** `vm-glpi`
- **Nombre :** 1

![04-lancer-details](https://hackmd.io/_uploads/SynLcH_AMx.jpg)

### 4.2 Source
- **Sélectionner la source de démarrage :** Image
- **Créer un nouveau volume :** Non (le disque du gabarit fait déjà 50 Go)
- Choisir l'image **Debian 13 trixie** (flèche ↑)

![05-lancer-source](https://hackmd.io/_uploads/Hyk_qHORMe.jpg)


### 4.3 Gabarit (flavor)
Choisir **`a4-ram8-disk50-perf1`** :
- `a4` → 4 vCPU
- `ram8` → 8 Go de RAM
- `disk50` → 50 Go de disque

![06-lancer-gabarit](https://hackmd.io/_uploads/rkZFqHuCfe.jpg)


### 4.4 Réseaux
Sélectionner **`ext-net1`** : la VM reçoit directement une adresse IP publique.

![07-lancer-reseaux](https://hackmd.io/_uploads/rJZ9cSuRzg.jpg)


### 4.5 Groupes de sécurité
- Ajouter **`glpi-sg`**

![08-lancer-groupes-securite](https://hackmd.io/_uploads/Bk7scrO0Ge.jpg)


### 4.6 Key Pair (paire de clés)
- Sélectionner **`cle-glpi`**

![09-lancer-key-pair](https://hackmd.io/_uploads/ryQ29HuAMe.jpg)


### 4.7 Lancement
Cliquer sur **Lancer l'instance**. Après environ 30 secondes, le statut passe à **Active** et l'adresse IP s'affiche.

![10-instance-active](https://hackmd.io/_uploads/H1_xoBO0zl.jpg)


---

## Étape 5 : Se connecter en SSH

Depuis **PowerShell** (Windows), dans le dossier contenant la clé :

```powershell
# Restreindre les droits du fichier (sinon SSH refuse la clé)
icacls cle-glpi.pem /inheritance:r /grant:r "$($env:USERNAME):R"

# Connexion
ssh -i cle-glpi.pem debian@<IP_PUBLIQUE>
```

À la première connexion, répondre `yes` pour accepter l'empreinte du serveur.

Vérifier les ressources de la VM :
```bash
nproc        # → 4
free -h      # → ~8 Go
df -h /      # → ~50 Go
cat /etc/debian_version   # → 13.x
```

📸 *Capture : connexion SSH réussie + résultat des commandes*
![](COLLER_CAPTURE_ICI)

---

## Étape 6 : Installer GLPI

### 6.1 Mise à jour et paquets (Apache, MariaDB, PHP)
```bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install -y apache2 mariadb-server php libapache2-mod-php wget \
  php-{mysql,curl,gd,intl,xml,mbstring,zip,bz2,ldap,apcu,bcmath}
```

### 6.2 Base de données
```bash
sudo mysql_secure_installation
sudo mysql -e "CREATE DATABASE glpi;
CREATE USER 'glpi'@'localhost' IDENTIFIED BY '<MOT_DE_PASSE>';
GRANT ALL ON glpi.* TO 'glpi'@'localhost';
FLUSH PRIVILEGES;"
```

### 6.3 Téléchargement de GLPI
Prendre la dernière version sur https://github.com/glpi-project/glpi/releases :
```bash
VER=11.0.x   # remplacer par la dernière version
wget https://github.com/glpi-project/glpi/releases/download/$VER/glpi-$VER.tgz
sudo tar xzf glpi-$VER.tgz -C /var/www/
sudo chown -R www-data:www-data /var/www/glpi
```

### 6.4 Configuration Apache
Fichier `/etc/apache2/sites-available/glpi.conf` :
```apache
<VirtualHost *:80>
    DocumentRoot /var/www/glpi/public
    <Directory /var/www/glpi/public>
        Require all granted
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
</VirtualHost>
```
```bash
sudo a2enmod rewrite
sudo a2dissite 000-default
sudo a2ensite glpi
sudo systemctl reload apache2
```

📸 *Capture : configuration Apache / `systemctl status apache2`*
![](COLLER_CAPTURE_ICI)

### 6.5 Assistant d'installation web
1. Ouvrir http://<IP_PUBLIQUE>
2. Choisir la langue, accepter la licence, cliquer sur **Installer**
3. Base de données : serveur `localhost`, utilisateur `glpi`, mot de passe choisi à l'étape 6.2
4. Sélectionner la base `glpi` et terminer l'installation

📸 *Capture : assistant GLPI + page de connexion*
![](COLLER_CAPTURE_ICI)

### 6.6 Sécurisation après installation
- Changer les mots de passe des comptes par défaut : `glpi`, `tech`, `normal`, `post-only`
- Supprimer le script d'installation :
```bash
sudo rm /var/www/glpi/install/install.php
```

📸 *Capture : tableau de bord GLPI*
![](COLLER_CAPTURE_ICI)

---

## Problèmes rencontrés / remarques

- Le lien Horizon `pub1` redirige vers `pub2` : il s'agit de la même interface.
- Sans clé SSH, impossible de se connecter : les images cloud Debian n'ont pas de mot de passe.
- Sans groupe de sécurité ouvrant les ports 80/443, la page GLPI ne s'affiche pas (timeout).