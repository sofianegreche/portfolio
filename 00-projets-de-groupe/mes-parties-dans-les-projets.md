# Mes parties dans les derniers projets

**Sofia Negreche** – Bachelor IT, classe E1A, Geneva Institute of Technology
GitHub : [@sofianegreche](https://github.com/sofianegreche)

---

## 1. Cartes de compétences (site web de recrutement)

### Le projet
Projet de groupe à 5 (Drilon, Deaa, Lilou, Art et moi). Le site présente chaque personne sous forme de **carte style FIFA** avec 6 statistiques (réseau, système, matériel, web, programmation, logique) qui montent grâce à des mini-jeux. Un **pourcentage de compatibilité** aide à recruter les bonnes personnes dans une équipe.

- **Technologies** : HTML, CSS, JavaScript en modules (sans framework), Firebase (Firestore)
- **Dépôt** : `dnhj-06/cartes-competences` (chef de projet : Drilon)
- **Méthode de travail** : une branche par sujet (`sofia/...`), jamais de commit sur `main`, pull requests en français relues par un autre membre, fusion en *Squash and merge*

### Mon rôle
Je suis **administratrice** du projet et **relectrice** des pull requests de Drilon et de Deaa. Je m'occupe de la protection des données, de la page Admin et de la page Mon équipe.

### Ce que j'ai réalisé

| PR | Sujet | Fichiers |
|----|-------|----------|
| #2 | Admin, mentions légales et accord photo | `admin.html`, `js/pages/admin.js`, `mentions-legales.html`, `docs/accord-photo.md`, `css/provisoire.css` |
| #25 | Mentions légales : photo facultative et retrait de l'accord | `mentions-legales.html` |
| #26 | Page Mon équipe | `equipe.html`, `js/pages/equipe.js`, `js/equipes-test.js` |
| #28 | Admin : « Pas de photo » quand `photoStatut` vaut `aucune` | `js/pages/admin.js` |

#### Mentions légales et accord photo (PR #2 et #25)
- Page de protection des données selon la **nLPD** (loi suisse), sans JavaScript.
- Texte de l'**accord photo** et phrase exacte de la case à cocher (`docs/accord-photo.md`).
- Décision du groupe que j'ai proposée : la photo est **facultative** (silhouette sans photo). Si un joueur **retire son accord**, sa photo et sa miniature sont supprimées et `photoStatut` repasse à `aucune`. Il peut redonner son accord plus tard.

#### Page Admin (PR #2 et #28)
- Liste des **photos en attente** avec boutons *Valider* / *Refuser*.
- **Tableau des joueurs** avec suppression et message de confirmation dans la page (pas de `alert()`).
- Correction : la colonne Photo affichait `undefined` pour un joueur sans photo, elle affiche maintenant « Pas de photo ».
- Testé au clavier, messages annoncés avec `role="status"`, listes vides gérées.

#### Page Mon équipe (PR #26)
- Affiche les équipes du joueur connecté : celles qu'il a **créées** (propriétaire) et celles dont il est **membre**.
- Pour chaque équipe : nom, places occupées (ex. 3/5), rôle, et chaque membre avec son **% de compatibilité** (`compatibilite.js`).
- Bouton **Retirer** visible seulement pour le propriétaire (et pas sur lui-même), avec message de confirmation et focus remis sur le titre de l'équipe.
- Message si le joueur n'a aucune équipe. Affichage avec `textContent` uniquement (jamais `innerHTML`) pour la sécurité.

### Ce que j'ai appris
- Travailler à plusieurs avec Git : branches, pull requests, relecture, résolution de conflits.
- Accessibilité (clavier, `role="status"`, focus) et protection des données (nLPD).
- Organiser un projet JavaScript en modules et suivre un format de données commun (`docs/format-donnees.md`).

---

## 2. AquaPark – jeu de course sur toboggan (Unity / C#)

### Le projet
Jeu 3D de course sur un **toboggan aquatique**, réalisé en groupe à 5 selon un cahier des charges. Chaque personne a une partie : joueur (Drilon), réapparition et arrivée (Lilou), obstacles/tremplin (Deaa), interface et score (Art), et **circuit et environnement : moi (Personne 2)**.

- **Technologies** : Unity, C#, package Splines
- **Dépôt** : `drrdh23/AquaPark-unity` – ma branche `feature/track`

### Mon rôle : Personne 2 – Circuit et environnement 3D

#### Première version (PR #4)
**Scripts**
- `TrackPath.cs` : le chemin du toboggan avec le package Splines. Il donne la position et la direction à une distance donnée (`GetPosition`, `GetDirection`), la largeur (`GetWidth(distance)` pour les parties étroites et larges), la longueur et la distance la plus proche d'un point (`GetNearestDistance`).
- `Checkpoint.cs` : quand le joueur (tag `Player`) traverse un checkpoint, il l'enregistre dans le `RespawnManager`.
- `Editor/TrackGenerator.cs` : menu **Tools > Personne 2 - Circuit > Générer le circuit** qui construit tout le circuit et met à jour le prefab.
- `TrackTester.cs` : capsule de test pour la scène `Test_Track`.

**Prefab `P_Track`** (prêt à placer dans `Level01`)
- Toboggan d'environ 1019 unités (~67 s), 6 couleurs, départ à 100 m sur une tour.
- Virages, descentes, petite montée, partie étroite (4 u) et large (9 u), bosse pour le tremplin.
- 12 checkpoints, une **KillZone** sous tout le parcours, ligne d'arrivée + piscine.
- Décor : sable, mer, piliers, palmiers, rochers, parasols et transats.

#### Deuxième version : montagnes russes (PR #6)
- Tracé d'environ **1345 unités** (~90 s), départ à 110 m : virages en S, bosses, **2 spirales**. J'ai vérifié que le tracé ne se croise jamais.
- **16 checkpoints**, chute maximale de 8,8 m entre deux checkpoints (sous la limite de 25 m du `RespawnManager`).
- **3 trous à sauter** (à 190, 690 et 1100) avec une KillZone basse et une **bande rouge** au sol pour indiquer quand sauter.
- Arrivée en bande plate pour ne pas cacher la caméra.
- Décor : mer, petites îles (sable, herbe, lagon), palmiers, rochers, grande île avec piscine.

**Tests réalisés** avec les vrais scripts du groupe (`PlayerMovement`, `PlayerCollision`, `RespawnManager`) :
- Sans sauter : chute dans le trou puis réapparition au checkpoint précédent
- Avec saut : les 3 trous passés, 16 checkpoints, arrivée détectée
- 0 erreur, 0 avertissement dans la Console

### Ce que j'ai appris
- Construire un circuit 3D avec les **Splines** de Unity et l'automatiser avec un script d'éditeur.
- Respecter un **contrat d'interface** commun pour que mon circuit fonctionne avec les scripts des autres.
- Tester l'intégration avec le travail du groupe avant de fusionner.

---

## 3. VM GLPI sur OpenStack (Infomaniak)

### Le projet
Créer une machine virtuelle sur le **Public Cloud Infomaniak (OpenStack)** pour héberger **GLPI**, l'outil de gestion de parc informatique et de tickets.

### Caractéristiques de la VM

| Élément | Valeur |
|---------|--------|
| Système | Debian 13 |
| Processeur | 4 vCPU |
| Mémoire | 8 Go RAM |
| Disque | 50 Go |
| Hébergeur | Infomaniak Public Cloud (OpenStack) |

### Ce que j'ai fait
1. **Connexion à Horizon**, l'interface web d'OpenStack chez Infomaniak.
2. **Paire de clés SSH** `cle-glpi` : la connexion à la VM se fait uniquement avec la clé, sans mot de passe.
3. **Groupe de sécurité** `glpi-sg` : règles pour SSH (22), HTTP (80) et HTTPS (443).
4. **Lancement de l'instance** : image Debian 13, gabarit 4 vCPU / 8 Go, disque 50 Go, réseau public, groupe `glpi-sg` et clé `cle-glpi`.
5. **Connexion en SSH** avec la clé (droits du fichier restreints, sinon SSH la refuse).
6. **Installation de GLPI** : Apache, MariaDB et PHP, création de la base `glpi`, téléchargement de GLPI, configuration d'Apache, puis assistant d'installation dans le navigateur.
7. **Sécurisation** après l'installation.

> Le compte rendu détaillé avec les captures est dans [`05-cloud/vm-debian-glpi-openstack.md`](../05-cloud/vm-debian-glpi-openstack.md).

### Ce que j'ai appris
- Créer et configurer une VM dans un cloud public (OpenStack).
- Installer une application web complète sur Linux (serveur web, PHP, base de données).
- Les bases de la sécurité d'un serveur exposé sur Internet.

---

## Autres projets
Mes autres comptes rendus (hardware, virtualisation, réseau, développement web) sont dans mon dépôt : [github.com/sofianegreche/portfolio](https://github.com/sofianegreche/portfolio)
