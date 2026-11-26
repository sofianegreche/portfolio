# Projet 3 – Migration et sauvegarde d’un poste de travail virtuel

> **Objectif :** Réinstaller complètement un système d’exploitation dans une machine virtuelle tout en conservant les données importantes de l’utilisateur grâce à une sauvegarde réalisée sur une clé USB.

---

# 1. Présentation du projet

Dans le cadre de ce projet, j’ai réalisé une migration sur une **machine virtuelle**.

L’objectif était de simuler la réinstallation complète d’un poste de travail tout en conservant les données importantes de l’utilisateur.

Les principales étapes étaient :

* identifier les données importantes à conserver ;
* réaliser une sauvegarde sur une clé USB ;
* vérifier que la sauvegarde était complète et accessible ;
* réinstaller proprement le système d’exploitation dans la machine virtuelle ;
* réinstaller les logiciels nécessaires ;
* restaurer les données sauvegardées ;
* vérifier le bon fonctionnement du système après la migration.

> **Particularité du projet :** la manipulation a été réalisée dans une machine virtuelle. Il n’était donc pas nécessaire de relever ou de configurer manuellement les paramètres réseau, l’adresse IP, le masque, la passerelle ou le DNS comme cela aurait été le cas sur un poste physique.

---

# 2. Inventaire avant migration

Avant de réinstaller le système, j’ai commencé par identifier les éléments importants à conserver.

## 2.1 Données personnelles

J’ai vérifié les différents dossiers contenant les données de l’utilisateur :

* `Documents`
* `Images`
* `Vidéos`
* `Téléchargements`
* `Bureau`
* autres dossiers contenant des fichiers importants.

Les fichiers nécessaires ont été sélectionnés afin de les inclure dans la sauvegarde.




---

## 2.2 Paramètres réseau

Sur un ordinateur physique, il aurait été possible de relever les paramètres réseau avant la réinstallation, notamment :

* adresse IP ;
* masque de sous-réseau ;
* passerelle ;
* DNS ;
* paramètres Wi-Fi ou Ethernet.

**Dans mon cas, cette étape n’était pas nécessaire**, car le projet a été réalisé dans une **machine virtuelle**. La configuration réseau est gérée par le logiciel de virtualisation et je n’avais donc pas besoin de relever ou de reconfigurer manuellement ces paramètres pour réaliser la migration.

---

## 2.3 Logiciels installés

J’ai identifié les logiciels nécessaires après la réinstallation.

Par exemple :

| Logiciel                     | Action         |
| ---------------------------- | -------------- |
| Navigateur Web               | Réinstallation |
| Suite bureautique            | Réinstallation |
| Lecteur PDF                  | Réinstallation |
| Outil de compression         | Selon besoin   |
| Autres logiciels nécessaires | Selon besoin   |

---

# 3. Sauvegarde des données

Une fois l’inventaire terminé, j’ai réalisé une sauvegarde des données importantes.

La sauvegarde a été effectuée sur une **clé USB** afin de conserver les fichiers en dehors de la machine virtuelle.

Les éléments sauvegardés comprenaient notamment :

* les documents personnels ;
* les images ;
* les vidéos ;
* les fichiers présents sur le Bureau ;
* les téléchargements importants ;
* les favoris du navigateur ;
* les autres fichiers nécessaires.

La sauvegarde a été réalisée **avant la réinstallation du système**.

### Emplacement de la sauvegarde

**Support utilisé :** Clé USB

**Contenu :** Données importantes de la machine virtuelle

**Date de la sauvegarde :** ....................................

---

# 4. Vérification de la sauvegarde

Avant de commencer la réinstallation, j’ai vérifié que les fichiers avaient correctement été copiés sur la clé USB.

J’ai notamment vérifié :

* la présence des dossiers principaux ;
* le nombre de fichiers sauvegardés ;
* la taille des données ;
* l’accès aux fichiers depuis la clé USB ;
* l’ouverture de plusieurs fichiers afin de vérifier qu’ils n’étaient pas corrompus ;
* la présence des favoris sauvegardés.

Cette vérification était importante afin de s’assurer que les données pourraient être restaurées après la réinstallation.

> **Point de vigilance :** la réinstallation ne doit commencer qu’après avoir vérifié que la sauvegarde est complète et accessible.

---

# 5. Réinstallation du système dans la machine virtuelle

Une fois la sauvegarde vérifiée, j’ai procédé à la réinstallation du système d’exploitation dans la machine virtuelle.

Les principales étapes sont les suivantes :

1. Vérifier une dernière fois que la sauvegarde présente sur la clé USB est accessible.
2. Démarrer la machine virtuelle sur le support d’installation du système.
3. Choisir l’installation personnalisée.
4. Supprimer ou formater les partitions nécessaires à l’ancienne installation.
5. Installer proprement le système d’exploitation.
6. Créer et configurer le compte utilisateur.
7. Installer les mises à jour nécessaires.
8. Vérifier que le système démarre correctement.

L'ancien système a ainsi été supprimé afin de repartir sur une installation propre.

---

# 6. Réinstallation des logiciels

Après l'installation du système d'exploitation, je vois que tout a été réinstaller automatiquement.

---

# 7. Restauration des données

Une fois le système réinstallé et les logiciels nécessaires disponibles, j’ai reconnecté la **clé USB contenant la sauvegarde** à la machine virtuelle.

Les données sauvegardées ont ensuite été copiées vers les emplacements appropriés du nouveau système :

* `Documents`
* `Images`
* `Vidéos`
* `Bureau`
* `Téléchargements`
* autres dossiers personnels.

Après la copie, plusieurs fichiers ont été ouverts afin de vérifier que les données avaient été correctement restaurées.

---




# 8. Configuration de la machine virtuelle

Contrairement à une migration réalisée sur un ordinateur physique, je n’ai pas eu besoin de reconfigurer manuellement les paramètres réseau.

La machine étant virtuelle, la connexion réseau était gérée par l’environnement de virtualisation.

Je n’ai donc pas eu à configurer :

* l'adresse IP ;
* le masque de sous-réseau ;
* la passerelle ;
* les serveurs DNS ;
* une connexion Wi-Fi physique.

L'objectif était principalement de vérifier que la machine virtuelle fonctionnait correctement après la réinstallation.

---

# 9 . Vérification finale

Après la migration, j’ai effectué une vérification générale de la machine virtuelle.

| Vérification                            | Résultat |
| --------------------------------------- | -------- |
| Système correctement installé           | ✅        |
| Système démarré correctement            | ✅        |
| Mises à jour installées                 | ✅        |
| Logiciels nécessaires installés         | ✅        |
| Données restaurées                      | ✅        |
| Documents accessibles                   | ✅        |
| Favoris restaurés                       | ✅        |
| Fichiers correctement ouverts           | ✅        |
| Connexion réseau de la VM fonctionnelle | ✅        |

La configuration d'une imprimante n'a pas été nécessaire dans le cadre de ce projet.

---

# 10. Bilan de la migration

Cette migration m'a permis de simuler la réinstallation complète d'un poste de travail dans une **machine virtuelle**, tout en conservant les données importantes de l'utilisateur.

Les principales opérations réalisées ont été :

1. Inventaire des données et logiciels.
2. Sauvegarde des données sur une clé USB.
3. Vérification de la sauvegarde.
4. Réinstallation propre du système dans la machine virtuelle.
5. Installation des logiciels nécessaires.
6. Restauration des données depuis la clé USB.
7. Restauration des favoris.
8. Vérification du fonctionnement de la machine virtuelle.

La sauvegarde réalisée avant la réinstallation a permis de limiter le risque de perte de données.

---

# 11. Procédure de migration réutilisable

## Phase 1 – Préparation

1. Identifier les données importantes.
2. Vérifier les dossiers personnels.
3. Identifier les navigateurs utilisés.
4. Sauvegarder les favoris.
5. Lister les logiciels nécessaires.
6. Vérifier les moyens de réinstallation des logiciels.

> Les paramètres réseau ne sont pas relevés dans cette procédure car la migration est réalisée dans une machine virtuelle.

## Phase 2 – Sauvegarde

7. Connecter la clé USB.
8. Copier les données importantes sur la clé USB.
9. Sauvegarder les favoris.
10. Vérifier la taille de la sauvegarde.
11. Vérifier la présence des fichiers.
12. Ouvrir plusieurs fichiers directement depuis la sauvegarde.
13. Ne poursuivre que lorsque la sauvegarde est validée.

## Phase 3 – Réinstallation

14. Démarrer la machine virtuelle sur le support d'installation.
15. Effectuer une installation personnalisée.
16. Formater les partitions concernées.
17. Installer le système d'exploitation.
18. Créer le compte utilisateur.
19. Installer les mises à jour.
20. Vérifier le démarrage correct du système.

## Phase 4 – Remise en service

21. Installer les logiciels nécessaires.
22. Reconnecter la clé USB.
23. Restaurer les données utilisateur.
24. Importer les favoris.
25. Vérifier le fonctionnement des logiciels.
26. Vérifier la connexion réseau de la machine virtuelle.

## Phase 5 – Contrôle

27. Vérifier l'accès aux documents.
28. Ouvrir plusieurs fichiers restaurés.
29. Vérifier les favoris.
30. Vérifier que les logiciels fonctionnent.
31. Vérifier le fonctionnement général de la machine virtuelle.
32. Effectuer une dernière vérification.

---

# 12. Points de vigilance

> ⚠️ **Sauvegarde avant réinstallation :** ne jamais commencer la réinstallation sans avoir vérifié la sauvegarde.

> ⚠️ **Clé USB :** vérifier que les fichiers sont réellement présents et accessibles sur la clé USB.

> ⚠️ **Favoris :** penser à sauvegarder les favoris avant de supprimer l'ancien système.

> ⚠️ **Logiciels :** vérifier avant la réinstallation que les logiciels nécessaires pourront être réinstallés.

> ⚠️ **Machine virtuelle :** contrairement à un poste physique, il n'est pas nécessaire de relever manuellement les paramètres réseau dans le cadre de ce projet.

> ⚠️ **Données importantes :** vérifier également les fichiers présents sur le Bureau et dans les Téléchargements.

> ⚠️ **Vérification finale :** ouvrir plusieurs fichiers après restauration afin de confirmer que les données sont exploitables.

---

# 13. Conclusion

Cette migration m'a permis de mettre en pratique une procédure complète de sauvegarde et de réinstallation d'un poste de travail, dans le contexte particulier d'une **machine virtuelle**.

La méthode suivie repose sur trois étapes principales :

**Inventorier → Sauvegarder et vérifier → Réinstaller et restaurer**

La sauvegarde sur clé USB a permis de conserver les données importantes avant la réinstallation. Après l'installation du nouveau système, les données ont pu être restaurées et vérifiées.

Le fait d'avoir travaillé dans une machine virtuelle a également simplifié la procédure, notamment parce qu'il n'était pas nécessaire de configurer manuellement les paramètres réseau ou les périphériques physiques.
