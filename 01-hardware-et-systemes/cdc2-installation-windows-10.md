### Cahier des charges 2

# Compte rendu — Installation et configuration de Windows 10 Pro

**Technicien : Negreche Sofia
**Nom du poste :** PC01_SN
**Système d’exploitation :** Windows 10 Pro
**Date d’installation :** 13/09/2026
**Mode de démarrage :** UEFI
**Type de partitionnement :** GPT

---

## 1. Introduction

L'objectif de ce travail est d'installer et de configurer un poste informatique sous **Windows 10 Pro**.

L'installation a été réalisée à partir d'un **fichier ISO de Windows 10**. Après l'installation du système, plusieurs configurations ont été effectuées afin de rendre le poste fonctionnel, sécurisé et prêt à l'emploi.

Les différentes étapes réalisées sont l'installation du système, la configuration du poste, la configuration du réseau, l'installation des mises à jour, l'optimisation du système, la vérification de la sécurité et les contrôles finaux.

Une vidéo accompagne ce compte rendu afin de présenter les différentes étapes réalisées pendant l'installation et la configuration.

---

# 2. Installation de Windows 10 Pro

Pour commencer, nous avons utilisé un **fichier ISO de Windows 10** afin d'effectuer l'installation du système d'exploitation.

Le PC a été démarré sur le support contenant l'image ISO de Windows 10. L'installation a ensuite été lancée.

Lors de l'installation, nous avons sélectionné les paramètres nécessaires, notamment la langue et la disposition du clavier.

Nous avons choisi l'édition **Windows 10 Pro** afin de respecter la consigne.

Le disque a été configuré en utilisant le format de partitionnement **GPT** et le démarrage a été effectué en **mode UEFI**.

Le mode UEFI associé au partitionnement GPT constitue une configuration moderne permettant au système d'exploitation de démarrer correctement sur la machine.

Une fois le disque configuré, l'installation de Windows 10 Pro a été lancée. Le programme d'installation a créé les partitions nécessaires et a copié les fichiers du système sur le disque.

Après plusieurs redémarrages, Windows 10 a terminé son installation et nous avons pu accéder au bureau.

---

# 3. Configuration initiale du système

Après l'installation, nous avons effectué la configuration initiale du système.

## 3.1 Nom du poste

Le nom du poste a été configuré selon la convention demandée :

**PC01_SN**

Les deux lettres `SN` correspondent aux initiales du technicien.

Ce nom permet d'identifier facilement le poste dans le réseau et dans l'infrastructure informatique.

---

## 3.2 Création du compte utilisateur

Un compte utilisateur a été créé pour permettre l'utilisation quotidienne du poste.

Le compte est protégé par un **mot de passe sécurisé**.

Le mot de passe n'est pas indiqué dans ce rapport pour des raisons de sécurité.

L'utilisation d'un compte utilisateur permet d'éviter de travailler systématiquement avec un compte administrateur et limite ainsi les risques liés aux modifications accidentelles ou aux logiciels malveillants.

---

## 3.3 Langue et clavier

La langue du système a été configurée en **français**.

La disposition du clavier utilisée est :

**Français (Suisse) — FR-CH**

Cette configuration correspond à la consigne et permet d'utiliser correctement la disposition d'un clavier suisse romand.

---

## 3.4 Fuseau horaire

Le fuseau horaire du système a été configuré sur :

**Europe/Zurich**

Cette configuration permet au système d'utiliser correctement l'heure locale et d'assurer une bonne synchronisation de l'heure du système.

---

# 4. Configuration du réseau

Une connexion réseau fonctionnelle a été configurée sur le poste.

Nous avons vérifié que l'ordinateur recevait correctement une adresse IP et qu'il pouvait communiquer avec le réseau.

Pour vérifier la configuration réseau, nous avons ouvert l'invite de commandes Windows et utilisé la commande :

```cmd
ipconfig
```

Cette commande permet notamment d'afficher l'adresse IPv4, le masque de sous-réseau et la passerelle par défaut.

La connexion réseau a été vérifiée afin de pouvoir notamment effectuer les mises à jour de Windows.

---

# 5. Windows Update

Une fois la connexion réseau fonctionnelle, nous avons effectué les mises à jour du système.

Pour cela, nous avons ouvert :

**Paramètres → Mise à jour et sécurité → Windows Update**

Nous avons lancé une recherche des mises à jour disponibles.

Les mises à jour proposées par Windows ont été téléchargées et installées.

Cette étape est importante car les mises à jour permettent notamment de corriger des failles de sécurité, des problèmes de fonctionnement et d'améliorer la stabilité du système.

Après l'installation des mises à jour, une nouvelle vérification a été effectuée afin de s'assurer que le système était à jour.

---

# 6. Optimisation du système

Après les mises à jour, nous avons effectué plusieurs opérations d'optimisation du poste.

## 6.1 Programmes au démarrage

Nous avons ouvert le **Gestionnaire des tâches**, puis la section **Applications de démarrage**.

Les programmes qui n'étaient pas nécessaires au démarrage du système ont été désactivés.

Cette opération permet de réduire le nombre de programmes lancés automatiquement au démarrage et peut améliorer le temps de démarrage de Windows.

Les programmes nécessaires au fonctionnement du système et à sa sécurité ont été conservés.

---

## 6.2 Plan d'alimentation

Nous avons vérifié le plan d'alimentation du PC dans les **Options d'alimentation** de Windows.

Le plan **Équilibré** a été utilisé car il permet d'obtenir un compromis entre les performances de la machine et sa consommation d'énergie.

Le choix du plan d'alimentation peut être adapté en fonction de l'utilisation du poste.

---

## 6.3 Nettoyage du disque

Un nettoyage du disque a également été effectué.

L'objectif était de supprimer les fichiers temporaires et les données inutiles afin de libérer de l'espace disque.

Cette opération permet de conserver un système plus propre et d'éviter l'accumulation de fichiers inutiles.

---

# 7. Configuration de la sécurité

## 7.1 Pare-feu Windows Defender

Nous avons vérifié que le **Pare-feu Microsoft Defender** était activé.

Le pare-feu permet de contrôler les communications réseau et constitue une première protection contre certaines connexions non autorisées.

L'état du pare-feu a été vérifié dans :

**Sécurité Windows → Pare-feu et protection réseau**

Le pare-feu est resté **actif**.

---

## 7.2 Windows Defender

Nous avons également vérifié que **Microsoft Defender Antivirus** était actif.

Cette protection permet de détecter et de bloquer différents types de logiciels malveillants.

Aucune désactivation de la protection antivirus n'a été effectuée.

Le système dispose donc de la protection antivirus intégrée de Windows.

---

# 8. Création d'un point de restauration

Après avoir terminé la configuration du système, nous avons créé un **point de restauration système**.

Cette opération a été effectuée à partir des paramètres de protection du système.

Le point de restauration permet de revenir à un état précédent du système en cas de problème logiciel.

Le point de restauration a été créé après les principales opérations de configuration afin de disposer d'un état fonctionnel du poste.

---

# 9. Vérification finale avec `winver`

Une vérification de la version de Windows a été effectuée à l'aide de la commande :

```text
winver
```

Cette commande permet d'afficher les informations concernant la version de Windows installée.

La vérification permet de confirmer que le système installé est bien :

**Windows 10 Pro**

Elle permet également d'identifier la version et le numéro de build du système.

---

# 10. Vérification finale du réseau avec `ipconfig`

Une dernière vérification de la configuration réseau a été réalisée avec :

```cmd
ipconfig
```

Cette commande nous a permis de confirmer que le poste disposait bien d'une configuration réseau fonctionnelle.

Les informations principales vérifiées sont :

* l'adresse IPv4 ;
* le masque de sous-réseau ;
* la passerelle par défaut.

Le poste dispose donc d'une connexion réseau opérationnelle.

---

# 11. Vérification des programmes au démarrage

Pour terminer l'optimisation du système, nous avons vérifié la liste des programmes configurés pour démarrer automatiquement avec Windows.

La vérification a été réalisée dans :

**Gestionnaire des tâches → Applications de démarrage**

Les applications inutiles ont été désactivées tandis que les applications nécessaires au fonctionnement du système ont été conservées.

Cette vérification permet de s'assurer que le système ne lance pas inutilement trop de programmes au démarrage.

---

# 12. Fiche de poste

| Élément                | Informations              |
| ---------------------- | ------------------------- |
| Nom du poste           | PC01_SN                   |
| Système d'exploitation | Windows 10 Pro            |
| Mode de démarrage      | UEFI                      |
| Partitionnement        | GPT                       |
| Langue                 | Français                  |
| Clavier                | Français (Suisse) — FR-CH |
| Fuseau horaire         | Europe/Zurich             |
| Adresse IP             | 192.168.31.129               |
| Date d'installation    | 13/09/2026                |
| Technicien responsable | Negreche Sofia                        |
| Windows Update         | Effectué                  |
| Pare-feu               | Actif                     |
| Windows Defender       | Actif                     |
| Nettoyage du disque    | Effectué                  |
| Point de restauration  | Créé                      |
| État du poste          | Opérationnel              |

---

# 13. Vérification finale

| Vérification                                | État |
| ------------------------------------------- | ---- |
| Windows 10 Pro installé                     | ✅    |
| Installation en UEFI                        | ✅    |
| Partitionnement GPT                         | ✅    |
| Nom du poste configuré                      | ✅    |
| Compte utilisateur créé                     | ✅    |
| Mot de passe sécurisé                       | ✅    |
| Langue française                            | ✅    |
| Clavier FR-CH                               | ✅    |
| Fuseau horaire Europe/Zurich                | ✅    |
| Connexion réseau fonctionnelle              | ✅    |
| Windows Update effectué                     | ✅    |
| Programmes inutiles au démarrage désactivés | ✅    |
| Plan d'alimentation configuré               | ✅    |
| Pare-feu Windows Defender actif             | ✅    |
| Windows Defender actif                      | ✅    |
| Nettoyage du disque effectué                | ✅    |
| Point de restauration créé                  | ✅    |
| `winver` vérifié                            | ✅    |
| `ipconfig` vérifié                          | ✅    |
| Applications au démarrage vérifiées         | ✅    |

---

# 14. Conclusion

L'installation de **Windows 10 Pro** a été réalisée à partir d'un fichier ISO en utilisant une configuration **UEFI avec un partitionnement GPT**.

Le poste a ensuite été configuré avec un nom respectant la convention demandée, un compte utilisateur protégé par un mot de passe sécurisé, la langue française, le clavier **FR-CH** et le fuseau horaire **Europe/Zurich**.

La connexion réseau a été configurée et vérifiée avec la commande `ipconfig`. Les mises à jour Windows ont ensuite été effectuées afin de disposer d'un système à jour.

Plusieurs opérations d'optimisation ont également été réalisées, notamment la désactivation des programmes inutiles au démarrage, la configuration du plan d'alimentation et le nettoyage du disque.

Enfin, la sécurité du poste a été vérifiée avec le **Pare-feu Windows Defender** et **Microsoft Defender Antivirus**, qui sont actifs. Un point de restauration système a également été créé.

Les vérifications finales avec `winver`, `ipconfig` et le Gestionnaire des tâches permettent de confirmer que le poste est correctement configuré.

Le poste **PC01_SN est donc opérationnel, à jour et prêt à l'emploi**.

video


# Compte rendu — Installation et configuration d'Ubuntu

**Technicien :** Negreche Sofia
**Nom du poste :** PC02_SN
**Système d'exploitation :** Ubuntu Desktop / Ubuntu Server
**Date d'installation :** 13/09/2026
**Adresse IP :** 192.168.31.128

---

# 1. Introduction

L'objectif de ce travail est d'installer et de configurer un poste informatique sous **Ubuntu**.

L'installation a été réalisée à partir d'un support contenant l'image ISO d'Ubuntu. Après l'installation du système, différentes configurations ont été effectuées afin de rendre le poste fonctionnel, sécurisé et prêt à l'emploi.

Les étapes réalisées comprennent :

* l'installation d'Ubuntu ;
* la création d'un compte utilisateur non-root ;
* la configuration de la langue et du clavier en **Français (Suisse) — FR-CH** ;
* la configuration du fuseau horaire ;
* la configuration et la vérification du réseau ;
* la mise à jour du système ;
* la suppression des paquets inutiles ;
* la vérification des services actifs ;
* l'activation et la configuration du pare-feu UFW ;
* les vérifications finales du système.

Une vidéo accompagne ce compte rendu afin de présenter les différentes étapes réalisées pendant l'installation et la configuration.

---

# 2. Installation d'Ubuntu

Pour commencer, nous avons démarré l'ordinateur à partir du support d'installation contenant l'image **ISO d'Ubuntu**.

Après le démarrage, nous avons lancé l'installation du système.

Selon l'attribution du formateur, l'installation a été réalisée avec **Ubuntu Desktop / Ubuntu Server**.

Lors de l'installation, nous avons sélectionné les paramètres nécessaires à la configuration du système.

Le disque a été préparé afin de permettre l'installation d'Ubuntu. Le programme d'installation a ensuite copié les fichiers nécessaires sur le disque.

Une fois l'installation terminée, le système a redémarré afin de démarrer sur le nouveau système Ubuntu.

---

# 3. Configuration initiale du système

Après le redémarrage, nous avons effectué la configuration initiale d'Ubuntu.

## 3.1 Création du compte utilisateur

Un compte utilisateur a été créé afin d'utiliser le système sans travailler directement avec le compte **root**.

Le compte possède un mot de passe sécurisé.

L'utilisation d'un compte non-root est une bonne pratique de sécurité. Lorsque des privilèges administrateur sont nécessaires, ils peuvent être obtenus temporairement avec la commande `sudo`.

Le mot de passe n'est pas indiqué dans ce rapport pour des raisons de sécurité.

---

## 3.2 Langue et clavier

La langue du système a été configurée en **français**.

La disposition du clavier utilisée est :

**Français (Suisse) — FR-CH**

Cette configuration permet d'utiliser correctement la disposition d'un clavier suisse romand.

---

## 3.3 Fuseau horaire

Le fuseau horaire du système a été configuré afin de correspondre à la zone :

**Europe/Zurich**

Cette configuration permet d'avoir une heure correcte sur le système et d'assurer une bonne synchronisation avec les services réseau.

---

# 4. Configuration du réseau

Une connexion réseau fonctionnelle a été configurée sur le poste.

Nous avons vérifié que l'ordinateur disposait bien d'une adresse IP et qu'il pouvait communiquer avec le réseau.

Pour vérifier les interfaces réseau et leurs paramètres, nous avons utilisé la commande :

```bash id="0b9f2r"
ip a
```

Cette commande permet d'afficher les interfaces réseau présentes sur le système ainsi que leurs adresses IP.

La connexion réseau est nécessaire notamment pour effectuer les mises à jour du système et installer ou supprimer des paquets.

---

# 5. Mise à jour du système

Une fois la connexion réseau vérifiée, nous avons effectué la mise à jour d'Ubuntu.

Les commandes demandées dans la consigne sont :

```bash id="7x1c8v"
sudo apt update && sudo apt upgrade
```

La commande `apt update` permet de mettre à jour la liste des paquets disponibles dans les dépôts configurés.

La commande `apt upgrade` permet ensuite d'installer les mises à jour disponibles pour les paquets déjà installés.

Cette étape permet notamment de corriger des problèmes de sécurité, des bugs et d'améliorer la stabilité du système.

Nous avons attendu la fin de la procédure afin de disposer d'un système à jour.

---

# 6. Suppression des paquets inutiles

Après les mises à jour, nous avons supprimé les paquets devenus inutiles à l'aide de la commande :

```bash id="ylz11q"
sudo apt autoremove
```

Cette commande permet de supprimer certains paquets qui avaient été installés comme dépendances mais qui ne sont plus nécessaires.

Cette opération permet de conserver un système plus propre et de libérer de l'espace disque.

---

# 7. Vérification des services

Nous avons ensuite vérifié les services présents et actifs sur le système.

Pour afficher les services actuellement en fonctionnement, nous avons utilisé :

```bash id="d2k6iq"
systemctl --type=service --state=running
```

Cette commande permet d'obtenir la liste des services actuellement actifs.

Nous avons également vérifié les services configurés pour être lancés automatiquement au démarrage du système avec :

```bash id="9t5a1c"
systemctl list-unit-files --type=service --state=enabled
```

Cette vérification permet d'identifier les services qui sont activés au démarrage.

Les services nécessaires au fonctionnement du système ont été conservés.

---

# 8. Configuration du pare-feu UFW

Afin d'améliorer la sécurité du poste, nous avons vérifié et configuré le pare-feu **UFW (Uncomplicated Firewall)**.

Nous avons tout d'abord vérifié son état avec :

```bash id="g3j7ww"
sudo ufw status
```

Si le pare-feu n'était pas actif, il a été activé avec :

```bash id="0g1c8f"
sudo ufw enable
```

Nous avons ensuite vérifié sa configuration avec :

```bash id="m6k6bq"
sudo ufw status verbose
```

Le pare-feu permet de contrôler les connexions réseau et de limiter les communications non autorisées.

### Cas d'un Ubuntu Server

Si la machine est administrée à distance avec SSH, il est nécessaire d'autoriser SSH avant d'activer le pare-feu afin de ne pas perdre la connexion :

```bash id="c0m8u4"
sudo ufw allow ssh
sudo ufw enable
```

Dans notre configuration, seules les règles nécessaires au fonctionnement du poste ont été autorisées.

---

# 9. Vérification de la version d'Ubuntu

Nous avons vérifié la version du système avec la commande :

```bash id="9f0i4x"
lsb_release -a
```

Cette commande permet d'afficher différentes informations concernant la distribution installée, notamment :

* le nom de la distribution ;
* la description ;
* la version ;
* le nom de code.

Cette vérification permet de confirmer que le système Ubuntu a été correctement installé.

---

# 10. Vérification finale du réseau

Une dernière vérification de la configuration réseau a été effectuée avec :

```bash id="2v3vnj"
ip a
```

Cette commande permet de vérifier que les interfaces réseau sont correctement configurées et qu'une adresse IP est attribuée.

### Informations relevées

```text
Adresse IP : _192.168.31.129_____________________
Interface réseau : _______________
```

La connexion réseau du poste est fonctionnelle.

---

# 11. Vérification finale des services activés au démarrage

Pour terminer, nous avons vérifié la liste des services configurés pour démarrer automatiquement avec Ubuntu.

La commande utilisée est :

```bash id="b4j5w2"
systemctl list-unit-files --type=service --state=enabled
```

Cette vérification permet de contrôler les services activés au démarrage et de détecter d'éventuels services inutiles.

Les services nécessaires au fonctionnement du système ont été conservés.

---

# 12. Fiche de poste

| Élément                    | Informations              |
| -------------------------- | ------------------------- |
| Nom du poste               | PC02_SN                   |
| Système d'exploitation     | Ubuntu Desktop / Server   |
| Langue                     | Français                  |
| Clavier                    | Français (Suisse) — FR-CH |
| Fuseau horaire             | Europe/Zurich             |
| Adresse IP                 | 192.168.31.129               |
| Compte utilisateur         | Compte non-root           |
| Date d'installation        | 13/09/2026                |
| Technicien responsable     | Negreche Sofia                       |
| Système mis à jour         | Oui                       |
| Paquets inutiles supprimés | Oui                       |
| Services vérifiés          | Oui                       |
| Pare-feu UFW               | Actif                     |
| État du poste              | Opérationnel              |

---

# 13. Commandes utilisées

Voici les principales commandes utilisées pendant la configuration :

```bash
# Mise à jour des dépôts et du système
sudo apt update && sudo apt upgrade

# Suppression des paquets inutiles
sudo apt autoremove

# Affichage des interfaces et adresses IP
ip a

# Affichage de la version Ubuntu
lsb_release -a

# Affichage des services actuellement actifs
systemctl --type=service --state=running

# Affichage des services activés au démarrage
systemctl list-unit-files --type=service --state=enabled

# Vérification du pare-feu
sudo ufw status

# Activation du pare-feu
sudo ufw enable

# Vérification détaillée du pare-feu
sudo ufw status verbose
```

---

# 14. Vérification finale

| Vérification                           | État |
| -------------------------------------- | ---- |
| Ubuntu installé                        | ✅    |
| Compte utilisateur non-root            | ✅    |
| Mot de passe sécurisé                  | ✅    |
| Langue française                       | ✅    |
| Clavier FR-CH                          | ✅    |
| Fuseau horaire Europe/Zurich           | ✅    |
| Connexion réseau fonctionnelle         | ✅    |
| `sudo apt update` effectué             | ✅    |
| `sudo apt upgrade` effectué            | ✅    |
| `sudo apt autoremove` effectué         | ✅    |
| Services actifs vérifiés               | ✅    |
| Services activés au démarrage vérifiés | ✅    |
| Pare-feu UFW activé                    | ✅    |
| `ip a` vérifié                         | ✅    |
| `lsb_release -a` vérifié               | ✅    |
| Poste opérationnel                     | ✅    |

---

# 15. Conclusion

L'installation et la configuration d'**Ubuntu** ont été réalisées conformément aux consignes.

Le système a été installé à partir d'une image ISO puis configuré avec un **compte utilisateur non-root**, la langue française, le clavier **FR-CH** et le fuseau horaire **Europe/Zurich**.

La connexion réseau a été configurée et vérifiée à l'aide de la commande `ip a`.

Le système a ensuite été mis à jour avec :

```bash
sudo apt update && sudo apt upgrade
```

Les paquets devenus inutiles ont été supprimés avec `sudo apt autoremove`.

Nous avons également vérifié les services actifs ainsi que les services configurés pour démarrer automatiquement avec le système.

Enfin, le pare-feu **UFW** a été vérifié et activé afin d'améliorer la sécurité du poste.

Les commandes `ip a`, `lsb_release -a` et `systemctl list-unit-files --type=service --state=enabled` ont permis d'effectuer les vérifications finales.

Le poste **PC02_SN est donc opérationnel, à jour, sécurisé et prêt à l'emploi**.

Video


