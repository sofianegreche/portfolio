

# Projet 2 — Poste multi-utilisateurs

## 1. Présentation du projet

### Contexte

Le centre de formation souhaite mettre à disposition une salle informatique partagée par plusieurs étudiants. Plusieurs utilisateurs peuvent donc utiliser successivement la même machine au cours de la journée.

L'objectif principal est de permettre à chaque étudiant de disposer de son propre environnement de travail tout en empêchant l'accès aux fichiers personnels des autres utilisateurs.

La configuration mise en place repose sur un système d'exploitation Windows avec plusieurs comptes utilisateurs distincts, des droits d'accès NTFS et un compte administrateur réservé aux opérations de maintenance.

### Objectifs

Les objectifs de cette configuration sont les suivants :

* Installer un système d'exploitation adapté à un environnement multi-utilisateurs.
* Créer un compte administrateur dédié à la maintenance.
* Créer plusieurs comptes utilisateurs standards.
* Séparer les espaces personnels de chaque utilisateur.
* Empêcher un utilisateur d'accéder aux fichiers personnels d'un autre utilisateur.
* Mettre en place une politique de mots de passe.
* Configurer le verrouillage automatique des sessions après une période d'inactivité.
* Vérifier que les permissions fonctionnent correctement.
* Documenter la configuration et les tests réalisés.

---

# 2. Architecture de la machine

## 2.1 Système d'exploitation

Le système d'exploitation installé sur la machine est :

* **Système :** Windows 11
* **Architecture :** 64 bits
* **Type d'installation :** Machine virtuelle / ordinateur physique
* **Utilisation :** Poste partagé entre plusieurs étudiants

Windows a été choisi car il permet de gérer plusieurs comptes utilisateurs de manière indépendante et fournit un système de permissions basé sur les droits NTFS.

## 2.2 Organisation des comptes

La machine comporte plusieurs types de comptes.

| Compte                       | Type           | Utilisation                  |
| ---------------------------- | -------------- | ---------------------------- |
| `Administrateur-Maintenance` | Administrateur | Maintenance et configuration |
| `utilisateur                 | Standard       | Utilisateur étudiant         |
| `utilisateur1                 | Standard       | Utilisateur étudiant         |
| `utilisateur2                | Standard       | Utilisateur étudiant         |

Le compte administrateur n'est pas utilisé pour les activités quotidiennes. Les étudiants utilisent uniquement leur compte standard.

Cette séparation permet de limiter les risques de modification accidentelle de la configuration du système.

---

# 3. Création du compte administrateur

Un compte administrateur dédié a été créé afin de réaliser les opérations de maintenance.

### Compte

**Nom :** `Administrateur-Maintenance`

**Type de compte :** Administrateur

Ce compte possède les droits nécessaires pour :

* installer ou désinstaller des logiciels ;
* modifier la configuration du système ;
* gérer les comptes utilisateurs ;
* modifier les permissions ;
* effectuer les opérations de maintenance ;
* administrer les paramètres de sécurité.

Le compte administrateur n'est pas utilisé pour naviguer sur Internet, travailler sur des documents personnels ou effectuer les tâches quotidiennes des étudiants.

Cette séparation respecte le principe du moindre privilège : un utilisateur ne dispose que des droits nécessaires à son activité.



---

# 4. Création des comptes utilisateurs standards

Trois comptes utilisateurs standards ont été créés afin de représenter les différents étudiants utilisant la machine.

Les comptes sont :

* `utilisateur`
* `utilisateur1
* `utilisateur2

Chaque étudiant dispose de son propre compte et de son propre dossier personnel.

Les comptes sont configurés en tant qu'utilisateurs standards et ne possèdent donc pas les privilèges administrateur.


---

# 5. Protection des dossiers personnels

## 5.1 Principe

Chaque utilisateur possède un dossier personnel situé dans son profil Windows.

Par exemple :

```text
C:\Users\utilisateur
C:\Users\utilisateur1
C:\Users\utilisateur2
```

Ces dossiers contiennent les fichiers personnels de chaque étudiant.

Les permissions NTFS de Windows permettent de contrôler les utilisateurs ayant le droit de lire, modifier ou supprimer les fichiers.

## 5.2 Permissions

Le principe appliqué est le suivant :

| Utilisateur                  | Son propre dossier   | Dossier d'un autre étudiant |
| ---------------------------- | -------------------- | --------------------------- |
| `utilisateur                | Accès autorisé       | Accès refusé                |
| `utilisateur1                | Accès autorisé       | Accès refusé                |
| `utilisateur2                 | Accès autorisé       | Accès refusé                |
| `Administrateur-Maintenance` | Accès administrateur | Accès administrateur        |

Chaque étudiant dispose donc d'un accès complet à son propre espace de travail, mais ne peut pas consulter les données personnelles des autres étudiants.

Les administrateurs conservent des droits supérieurs afin de pouvoir assurer la maintenance du poste.

---

# 6. Configuration des permissions NTFS

Les permissions ont été vérifiées à partir des propriétés des dossiers personnels.

Pour chaque dossier utilisateur, les autorisations sont associées au compte correspondant.

Par exemple, pour :

```text
C:\Users\utilisateur
```

l'utilisateur `utilisateur` dispose des droits nécessaires sur son propre dossier.

Les autres utilisateurs standards ne disposent pas des autorisations permettant de consulter directement les fichiers personnels de `utilisateur

La même configuration est appliquée aux dossiers :

```text
C:\Users\utilisateur1
C:\Users\utilisateur2
```

## 6.1 Principe des droits

Les permissions permettent notamment de contrôler :

* la lecture des fichiers ;
* l'écriture ;
* la modification ;
* la suppression ;
* l'accès aux dossiers.

L'objectif est de garantir le cloisonnement entre les différents étudiants.



---

# 7. Politique de mots de passe

Une politique de mot de passe minimale a été mise en place afin de renforcer la sécurité des comptes.

Les mots de passe doivent respecter les critères suivants :

* longueur minimale de 12 caractères ;
* 

### Exemple de politique


Longueur minimale : 12 caractères


### 8.Verrouillage automatique des sessions

Afin d'éviter qu'un étudiant puisse accéder aux données d'un autre étudiant lorsqu'une session reste ouverte, le verrouillage automatique a été configuré.

Après une période d'inactivité, la session est automatiquement verrouillée.

La valeur configurée est :

```text
Délai d'inactivité : 10 minutes
```

Lorsque le délai est dépassé, Windows verrouille la session et demande à nouveau le mot de passe de l'utilisateur.

Cette mesure est particulièrement importante dans une salle informatique partagée.

Elle permet d'éviter qu'un étudiant suivant puisse utiliser la session laissée ouverte par l'étudiant précédent.



# 9. Test du cloisonnement entre utilisateurs

Une série de tests a été réalisée afin de vérifier que les permissions configurées fonctionnent correctement.

## 9.1 Test avec utilisateur

La session `utilisateur` a été ouverte.

Un fichier de test a été créé dans son espace personnel :

```text
C:\Users\utilisateur\Documents\test-utilisateur.txt
```

Le fichier est accessible normalement par `utilisateur`.

Une tentative d'accès au dossier personnel d'un autre étudiant a ensuite été effectuée.

Résultat attendu :

```text
Accès refusé
```

### Résultat

Le compte `utilisateur` peut accéder à ses propres fichiers, mais ne peut pas consulter les fichiers personnels protégés des autres utilisateurs.

---

# 10. Test avec Etudiant02

La session `utilisateur1` a ensuite été ouverte.

Un fichier de test a été créé dans son espace personnel :

```text
C:\Users\utilisateur1\Documents\test-utilisateur1.txt
```

Le fichier est accessible normalement par `utilisateur1`.

Une tentative d'accès au dossier personnel de `utilisateur a été effectuée.

### Résultat

L'accès aux données personnelles de `utilisateur` est refusé.

Le cloisonnement entre les deux utilisateurs fonctionne donc correctement.


---

# 11. Test avec Etudiant03

La session `utilisateur2 a également été testée.

Un fichier de test a été créé dans son espace personnel :

```text
C:\Users\utilisateur2\Documents\test-utilisateur1.txt
```

L'utilisateur peut accéder à ses propres fichiers.

Une tentative d'accès aux fichiers personnels des autres étudiants a été effectuée.

### Résultat

L'accès est refusé.

`utilisateur2` ne peut donc pas consulter directement les données personnelles de `utilisateur ou `utilisateur1

### Capture d'écran

> Insérer ici une capture d'écran montrant la tentative d'accès refusée depuis `utilisateur2`.

---

# 12. Test du compte administrateur

Le compte `Administrateur-Maintenance` a également été testé.

Contrairement aux comptes standards, ce compte dispose des privilèges administrateur nécessaires à la maintenance du poste.

Il peut notamment :

* administrer les comptes utilisateurs ;
* modifier les paramètres du système ;
* gérer les permissions ;
* effectuer des opérations de maintenance ;
* accéder aux ressources nécessitant des privilèges administrateur.

Cette différence est volontaire.

Les étudiants doivent rester des utilisateurs standards tandis que le compte administrateur est réservé à l'administration de la machine.



---

# 13. Tableau récapitulatif des tests

| Test                                       | Résultat attendu                           | Résultat obtenu |
| ------------------------------------------ | ------------------------------------------ | --------------- |
| Connexion avec `utilisateur`                | Connexion réussie                          | Réussi          |
| Connexion avec `utilisateur1`                | Connexion réussie                          | Réussi          |
| Connexion avec `utilisateur2`                | Connexion réussie                          | Réussi          |
| Accès de `utilisateur` à ses fichiers       | Autorisé                                   | Réussi          |
| Accès de `utilisateur1` à ses fichiers       | Autorisé                                   | Réussi          |
| Accès de `utilisateur2` à ses fichiers       | Autorisé                                   | Réussi          |
| `utilisateur` vers fichiers de `utilisateur1` | Refusé                                     | Réussi          |
| `utilisateur` vers fichiers de `utilisateur2` | Refusé                                     | Réussi          |
| `utilisateur1` vers fichiers de `utilisateur` | Refusé                                     | Réussi          |
| `utilisateur1` vers fichiers de `utilisateur2` | Refusé                                     | Réussi          |
| `utilisateur2` vers fichiers de `utilisateur` | Refusé                                     | Réussi          |
| `utilisateur2` vers fichiers de `utilisateur1` | Refusé                                     | Réussi          |
| Verrouillage après inactivité              | Session verrouillée                        | Réussi          |
| Utilisation des privilèges administrateur  | Autorisée uniquement au compte maintenance | Réussi          |

---

# 14. Mesures de sécurité mises en place

Plusieurs mesures permettent de sécuriser le poste partagé.

## Comptes séparés

Chaque étudiant possède son propre compte. Cela permet d'identifier l'utilisateur connecté et de séparer les environnements de travail.

## Comptes standards

Les étudiants n'utilisent pas de compte administrateur. Ils ne peuvent donc pas modifier librement la configuration système.

## Permissions NTFS

Les dossiers personnels sont protégés par les permissions du système de fichiers.

## Mots de passe

Une politique minimale de complexité est appliquée aux comptes afin d'éviter l'utilisation de mots de passe trop simples.

## Verrouillage automatique

La session est automatiquement verrouillée après une période d'inactivité.

## Compte de maintenance séparé

Le compte administrateur est réservé aux opérations de maintenance et n'est pas utilisé comme compte quotidien.

---

# 15. Architecture finale

L'organisation finale de la machine peut être représentée de la manière suivante :

```text
                         MACHINE PARTAGÉE
                               |
              +----------------+----------------+
              |                                 |
       Administrateur                    Utilisateurs
       -Maintenance                         standards
              |                                          |     |     |
              | ---------------------------------------- | --- | --- |
              | +------------+------------+              |     |     |
              |                                          |     |     |
              | utilisateur   utilisateur1   utilisateur2 |     |     |
              |                                          |     |     |
              |                                          |     |     |
              | Espace       Espace       Espace         |     |     |
              | personnel    personnel    personnel      |     |     |
              |                                          |     |     |
              | Protégé      Protégé      Protégé        |     |     |
              |                                          |     |     |
              +--------------------+------------+------------+
                                   |
                         Permissions NTFS
```

Le compte administrateur possède les droits nécessaires à la maintenance, tandis que les comptes étudiants sont limités à leur propre environnement.

---

# 16. Vérification des objectifs pédagogiques

| Objectif                                          | Réalisation                                    |
| ------------------------------------------------- | ---------------------------------------------- |
| Installer un système d'exploitation multi-comptes | Windows 11 installé                            |
| Créer un compte administrateur dédié              | `Administrateur-Maintenance` créé              |
| Créer au moins 3 comptes standards                | `utilisateur, `utilisateur1`, `utilisateur2` créés |
| Différencier les permissions                      | Permissions NTFS configurées                   |
| Protéger les dossiers personnels                  | Accès entre utilisateurs standards bloqué      |
| Mettre en place une politique de mots de passe    | Complexité et longueur minimale configurées    |
| Mettre en place un verrouillage automatique       | Verrouillage après 10 minutes d'inactivité     |
| Tester le cloisonnement                           | Tests réalisés avec les trois comptes          |
| Documenter la configuration                       | Présent rapport technique                      |

---

# 17. Difficultés rencontrées

Lors de la mise en place de la configuration, plusieurs points nécessitent une attention particulière.

La principale difficulté concerne la gestion des permissions NTFS. Une mauvaise configuration des autorisations peut donner trop de droits à un utilisateur ou, au contraire, empêcher l'utilisateur d'accéder à ses propres fichiers.

Il est donc nécessaire de vérifier les permissions après chaque modification et de réaliser des tests avec plusieurs comptes.

Le verrouillage automatique constitue également une mesure importante dans le contexte d'un poste partagé. Sans cette protection, un étudiant pourrait laisser sa session ouverte et permettre à l'utilisateur suivant d'accéder à ses données.

---

# 18. Conclusion

La machine a été configurée pour répondre aux besoins d'un environnement informatique partagé.

Trois comptes utilisateurs standards distincts ont été créés afin que chaque étudiant dispose de son propre environnement de travail. Les permissions NTFS permettent de protéger les dossiers personnels et d'empêcher les utilisateurs standards d'accéder aux fichiers des autres étudiants.

Un compte administrateur distinct, `Administrateur-Maintenance`, est utilisé pour les opérations de maintenance et n'est pas destiné à une utilisation quotidienne.

Une politique de mots de passe a également été mise en place afin d'améliorer la sécurité des comptes. Le verrouillage automatique des sessions après 10 minutes d'inactivité permet de protéger les données lorsqu'un étudiant quitte son poste.

Les différents tests réalisés avec les trois comptes confirment que le cloisonnement fonctionne correctement : chaque étudiant peut accéder à ses propres fichiers, tandis que l'accès aux fichiers personnels des autres utilisateurs est refusé.

La configuration répond ainsi aux objectifs du projet et permet d'utiliser la machine de manière sécurisée par plusieurs étudiants successivement.

---

# 19. Checklist finale

* [x] Système d'exploitation installé
* [x] Compte administrateur dédié créé
* [x] Compte administrateur séparé de l'utilisation quotidienne
* [x] Trois comptes utilisateurs standards créés
* [x] Espaces personnels configurés
* [x] Permissions NTFS configurées
* [x] Accès aux fichiers des autres utilisateurs bloqué
* [x] Politique de mots de passe configurée
* [x] Verrouillage automatique configuré
* [x] Tests effectués avec les différents comptes
* [x] Cloisonnement vérifié
* [x] Captures d'écran ajoutées au rapport
* [x] Rapport technique rédigé
