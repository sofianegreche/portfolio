# Compte rendu – Projet 2 : Snapshots et plan de reprise d'une VM critique

9 octobre 2026 · Negreche Sofia · E1A

## Contexte et objectifs

Le serveur applicatif virtualisé d'une entreprise doit pouvoir être restauré rapidement en cas de mauvaise mise à jour ou de panne. Ce TP a été réalisé sur **SVR-APPLI**, le serveur web Apache (Ubuntu) du Projet 1, hébergé sur VMware Workstation.

Objectifs :

- Comprendre le mécanisme des snapshots (instantanés).
- Mettre en place une politique de snapshots réguliers.
- Simuler un incident et restaurer la VM à partir d'un snapshot.
- Documenter un plan de reprise simple.

**Résultat : la VM a été restaurée en 22 secondes après un incident.** Les 4 objectifs sont atteints.

## Environnement de test

| Élément | Valeur |
| --- | --- |
| Hyperviseur | VMware Workstation |
| VM critique | SVR-APPLI (Ubuntu 26.04 LTS), 192.168.208.12 |
| Application | Serveur web Apache (port 80), page d'accueil « Projet 2 » |
| Poste de test | SVR-IMPRESSION (Windows 11), navigateur Edge |
| Réseau | NAT (VMnet8), 192.168.208.0/24 |

## Principe d'un snapshot

Un snapshot fige l'état d'une VM à un instant donné : disque, configuration et, si la VM est allumée, mémoire vive. Après le snapshot, VMware écrit les modifications dans un fichier séparé (disque delta). Restaurer le snapshot revient à jeter ce delta : la VM revient exactement à l'état figé.

Un snapshot **n'est pas une sauvegarde** : il dépend du disque d'origine et est stocké au même endroit que la VM. Si le disque du PC hôte tombe en panne, la VM et ses snapshots sont perdus ensemble.

## Étape 1 : état sain de départ

La page d'accueil d'Apache a été mise à jour pour indiquer « Projet 2 » :

```bash
echo "<h1>SVR-APPLI - Serveur applicatif PME</h1><p>Projet 2 - Negreche Sofia - E1A</p>" | sudo tee /var/www/html/index.html
```

![image](https://hackmd.io/_uploads/Hk-0mN8iMg.png)


## Étape 2 : prise du snapshot

VM allumée, menu **VM → Snapshot → Prendre un snapshot** :

- Nom : `AVANT-MAJ-09-10`
- Description : « État sain avant mise à jour, Apache fonctionnel »
- Créé le 09.10.2026 à 10:56:59

![image](https://hackmd.io/_uploads/ByqyVVUjfg.png)


Le gestionnaire de snapshots montre la chaîne : état d'origine → AVANT-MAJ-09-10 → « Vous êtes ici » (état actuel).

![image](https://hackmd.io/_uploads/BJQW4N8ofx.png)


## Étape 3 : donnée créée après le snapshot

Pour mesurer la perte de données (RPO), une « commande client » a été créée **après** le snapshot :

```bash
echo "Commande client n°42 - créée après le snapshot" | sudo tee /var/www/html/commande.txt
```

![image](https://hackmd.io/_uploads/S12fNVLizx.png)


Elle est accessible depuis SVR-IMPRESSION sur `http://192.168.208.12/commande.txt`. Les accents s'affichent mal à cause de l'encodage du navigateur, sans incidence sur le test.

![image](https://hackmd.io/_uploads/Sy1VVVUizx.png)


## Étape 4 : simulation de l'incident

Une mise à jour ratée a été simulée : suppression de la page d'accueil, puis désinstallation d'Apache.

```bash
sudo rm /var/www/html/index.html
sudo apt remove apache2 -y
```

Le serveur web n'est plus disponible : le site est hors service.

![image](https://hackmd.io/_uploads/Hym_4VUizx.png)


## Étape 5 : restauration et mesure du temps

Menu **VM → Snapshot → Restaurer le snapshot : AVANT-MAJ-09-10**. VMware prévient que l'état actuel sera perdu.

![image](https://hackmd.io/_uploads/HkqKVEUoze.png)


Le temps a été chronométré du clic sur « Oui » jusqu'à l'affichage de la page d'accueil dans Edge sur SVR-IMPRESSION.

| Mesure | Résultat |
| --- | --- |
| Temps de restauration | **22 secondes** |
| Page d'accueil « Projet 2 » | De nouveau accessible |
| Service Apache | De nouveau actif |

![image](https://hackmd.io/_uploads/r1ejNEIjGl.png)


Le terminal de SVR-APPLI est revenu exactement à l'état du snapshot : la mise à jour de la page y figure, mais plus la création de `commande.txt`, ni la suppression, ni la désinstallation d'Apache.

![image](https://hackmd.io/_uploads/ryS34NIjfx.png)


La commande n°42 a disparu : Apache répond bien, mais renvoie une erreur 404. C'est la perte de données liée à la restauration (RPO).

![image](https://hackmd.io/_uploads/rywTNN8iGl.png)


## Étape 6 : politique de snapshots réguliers

**AutoProtect** a été activé sur SVR-APPLI (VM → Paramètres → Options → AutoProtect) :

| Paramètre | Valeur |
| --- | --- |
| Intervalle | Horaire |
| Snapshots AutoProtect maximum | 3 |
| Rotation | 1 snapshot de l'heure, 1 du jour, 1 de la semaine |
| Espace disque nécessaire | Au moins 12 Go |

![image](https://hackmd.io/_uploads/BJ_C4EIszl.png)


En plus d'AutoProtect, la règle est de prendre un **snapshot manuel avant toute mise à jour** ou modification importante, comme dans ce TP.

## Plan de reprise – SVR-APPLI

**Objectif :** remettre en service le serveur applicatif après une mauvaise mise à jour ou une panne logicielle.

### Objectifs de reprise

| Indicateur | Définition | Valeur retenue |
| --- | --- | --- |
| RTO | Durée maximale d'interruption acceptable | **15 minutes** (restauration mesurée : 22 s, plus détection, décision et vérification) |
| RPO | Perte de données maximale acceptable | **1 heure** (snapshots AutoProtect horaires) ; **0** pour une mise à jour planifiée précédée d'un snapshot manuel |

### Politique de snapshots

- Snapshot manuel avant chaque mise à jour, nommé `AVANT-MAJ-JJ-MM`.
- AutoProtect horaire, 3 snapshots conservés (heure, jour, semaine).
- Suppression des snapshots manuels quand la mise à jour est validée (au plus quelques jours), car une longue chaîne de snapshots ralentit la VM et consomme du disque.

### Procédure de reprise

1. **Détecter** : le site ne répond plus ou une erreur apparaît après une mise à jour.
2. **Décider** : vérifier si une réparation rapide est possible ; sinon, décider la restauration.
3. **Prévenir** les utilisateurs que le service est interrompu et que les données saisies depuis le dernier snapshot seront perdues.
4. **Restaurer** : VMware → VM → Snapshot → Gestionnaire de snapshots → choisir le snapshot sain le plus récent → Restaurer.
5. **Vérifier** : ouvrir `http://192.168.208.12` depuis un poste client ; contrôler que la page s'affiche.
6. **Ressaisir** les données perdues si possible (commandes créées après le snapshot).
7. **Documenter** l'incident : date, cause, snapshot utilisé, durée d'interruption.

### Rôles

| Rôle | Responsabilité |
| --- | --- |
| Administrateur système | Restauration et vérification |
| Responsable de la PME | Validation de la décision de restaurer |
| Utilisateurs | Signalement de l'incident, ressaisie des données perdues |

### Limites et améliorations

- Le snapshot protège contre une erreur logicielle, pas contre la panne du PC hôte : il faut une **vraie sauvegarde** de la VM sur un autre support (disque externe, NAS), idéalement selon la règle 3-2-1 (3 copies, 2 supports, 1 hors site).
- Toute donnée saisie après le dernier snapshot est perdue : pour un serveur avec des données importantes, il faut sauvegarder les données séparément et plus souvent.
- AutoProtect demande au moins 12 Go d'espace disque libre sur l'hôte.

## Bilan

| Livrable | État |
| --- | --- |
| Preuve du snapshot | OK (captures 02 et 03) |
| Incident simulé | OK (capture 06) |
| Restauration réussie | OK (captures 08 et 09) |
| Mesure du temps de restauration | OK : 22 secondes |
| Politique de snapshots réguliers | OK : AutoProtect horaire |
| Plan de reprise | OK (section ci-dessus) |

## Conclusion

Le snapshot permet de revenir en 22 secondes à un état sain après une mauvaise mise à jour, ce qui est très rapide par rapport à une réinstallation. Le test montre aussi sa limite : les données créées après le snapshot sont perdues. En production, les snapshots servent donc à sécuriser les mises à jour, et doivent être complétés par de vraies sauvegardes.