### Projet 1

# Fiche de mise en service – Poste de travail du cabinet comptable

**Projet :** Mise en service d'un poste de travail pour une nouvelle collaboratrice
**Entreprise :** Petit cabinet comptable – 3 personnes
**Système d'exploitation :** Windows 11
**Type de poste :** machine virtuelle
**Utilisateur :** Nouvelle collaboratrice
**Date de mise en service :** 14/09/2026
**Technicien :** Negreche Sofia

---

# 1. Objectif de l'intervention

L'objectif de cette intervention était de préparer un poste de travail informatique destiné à une nouvelle collaboratrice d'un cabinet comptable.

Le poste devait être opérationnel dès son arrivée, avec :

* Un système d'exploitation fonctionnel ;
* Un compte administrateur local ;
* Un compte utilisateur standard sécurisé ;
* Une suite bureautique ;
* Un navigateur Internet ;
* Un lecteur PDF ;
* Un accès à l'imprimante réseau partagée ;
* Les mises à jour du système activées ;
* Un antivirus actif ;
* Le pare-feu activé ;
* Une configuration réseau fonctionnelle.

L'objectif était également d'appliquer les premières mesures de sécurité informatique afin de protéger le poste et les données professionnelles.

---

# 2. Installation du système d'exploitation

## 2.1 Préparation

Le système d'exploitation choisi pour ce poste est **Windows 11**.

L'installation a été réalisée à partir du support d'installation fourni, sous forme de clé USB ou d'image ISO selon l'environnement utilisé.

Avant l'installation, les éléments suivants ont été vérifiés :

* Disponibilité du support d'installation ;
* Compatibilité de la machine avec Windows 11 ;
* Espace disque disponible ;
* Connexion réseau ;
* Possibilité d'effectuer les mises à jour après l'installation.

## 2.2 Installation

Les étapes suivantes ont été réalisées :

1. Démarrage de la machine sur le support d'installation ;
2. Sélection de la langue ;
3. Sélection du clavier et du format régional ;
4. Lancement de l'installation de Windows 11 ;
5. Sélection du disque destiné au système ;
6. Installation des fichiers Windows ;
7. Redémarrage de la machine ;
8. Configuration initiale de Windows ;
9. Connexion au réseau ;
10. Arrivée sur le bureau Windows.

L'installation s'est terminée correctement et le système est fonctionnel.

---

# 3. Création et configuration des comptes utilisateurs

Deux comptes locaux ont été configurés afin de séparer les tâches d'administration de l'utilisation quotidienne du poste.

## 3.1 Compte administrateur

Un compte administrateur local a été créé afin de permettre les opérations nécessitant des privilèges élevés, notamment :

* L'installation de logiciels ;
* L'installation de pilotes ;
* La modification de certains paramètres système ;
* La maintenance du poste ;
* L'administration du système.

Ce compte n'est pas destiné à l'utilisation quotidienne de la collaboratrice.

**Nom du compte administrateur :** `Administrator`

Le mot de passe administrateur respecte une politique de sécurité comprenant :

* Une longueur suffisante ;
* Des lettres majuscules et minuscules ;
* Des chiffres ;
* Des caractères spéciaux ;
* Aucun élément facilement devinable.

---

## 3.2 Compte utilisateur standard

Un deuxième compte a été créé pour la nouvelle collaboratrice.

**Nom du compte :** `utilisateur

Ce compte possède le statut **Utilisateur standard** et ne dispose pas des privilèges administrateur.

Cette configuration permet de limiter les modifications pouvant être effectuées sur le système lors de l'utilisation quotidienne du poste.

Le compte utilisateur est protégé par un mot de passe conforme à la politique de sécurité définie.

### Politique de mot de passe appliquée

Le mot de passe doit :

* Contenir au minimum 8 caractères ;
* Comprendre des majuscules et minuscules ;
* Contenir au moins un chiffre ;
* Contenir au moins un caractère spécial ;
* Ne pas contenir le nom de l'utilisateur ;
* Ne pas être un mot de passe courant ou facilement devinable.

### Vérification

Une connexion avec le compte standard a été effectuée afin de vérifier que :

* Le compte permet l'utilisation normale de Windows ;
* Les logiciels peuvent être lancés ;
* L'accès au réseau fonctionne ;
* Les opérations nécessitant les droits administrateur demandent une authentification administrateur.

---

# 4. Configuration du réseau

Le poste a été connecté au réseau local du cabinet comptable.

## 4.1 Nom de la machine

Un nom identifiable a été attribué au poste.

**Nom de la machine :** `PC01_w11

Ce nom permet d'identifier facilement le poste sur le réseau.

## 4.2 Configuration réseau

La connexion réseau a été vérifiée et permet :

* L'accès au réseau local ;
* L'accès à Internet ;
* La communication avec les ressources partagées ;
* L'accès à l'imprimante réseau.

La configuration IP a été réalisée conformément à l'environnement utilisé.

**Adresse IP :** 192.168.208.129
**Masque :** 255.255.255.0
**Passerelle :** 192.168.208.2


Dans le cas d'une configuration DHCP, l'adresse IP est attribuée automatiquement par le serveur DHCP.

## 4.3 Groupe de travail

Le poste a été configuré dans le groupe de travail du réseau local.

**Groupe de travail :** `WORKGROUP`

Cette configuration permet au poste de fonctionner correctement dans le réseau local du cabinet.

---

# 5. Installation des logiciels

Les logiciels nécessaires au fonctionnement quotidien du poste ont été installés.

## 5.1 Suite bureautique

La suite bureautique libre **LibreOffice** a été installée afin de permettre à la collaboratrice de réaliser les tâches bureautiques nécessaires à son activité.

LibreOffice comprend notamment :

* **Writer** pour la création et la modification de documents texte ;
* **Calc** pour la création et la modification de tableaux ;
* **Impress** pour la création de présentations ;
* **Draw** pour la création et la modification de documents graphiques ;
* **Base** pour la gestion de bases de données.

L'installation de LibreOffice a été réalisée correctement et les applications ont été testées afin de vérifier leur bon fonctionnement.

## 5.2 Navigateur Internet

Un navigateur Internet est disponible sur le poste afin de permettre l'accès aux sites professionnels et aux services en ligne.

**Navigateur utilisé :** Microsoft Edge

Le navigateur a été vérifié afin de confirmer son bon fonctionnement et l'accès à Internet.

## 5.3 Lecteur PDF

Le lecteur PDF intégré à Microsoft Edge permet de consulter les documents PDF nécessaires à l'activité professionnelle.

Aucun logiciel PDF supplémentaire n'a donc été installé.

La lecture d'un document PDF a été testée afin de vérifier que les fichiers peuvent être ouverts correctement.
