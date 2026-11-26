# Projet 4 — Rapport de migration P2V de SVR-FICHIERS

Negreche Sofia · Classe E1A · Cours Virtualisation · 11.10.2026

## 1. Contexte et objectifs

Le serveur de fichiers SVR-FICHIERS a été migré avec succès vers une nouvelle machine virtuelle, SVR-FICHIERS-P2V, sans perte de service ni de données.

Une entreprise souhaite virtualiser un ancien serveur physique pour prolonger sa durée de vie logicielle et simplifier sa maintenance. Dans ce projet, la VM SVR-FICHIERS (créée au Projet 1) joue le rôle de cet ancien serveur physique. La migration est donc simulée, mais la procédure et les outils utilisés sont ceux d'une vraie migration P2V (Physical to Virtual).

Objectifs :

- comprendre le principe d'une migration P2V ;
- réaliser la migration avec un outil de capture de disque ;
- vérifier que le serveur fonctionne de la même façon après migration ;
- documenter les problèmes, les limites et les risques rencontrés.

Environnement : VMware Workstation, segment LAN isolé LABO-ISOLE (10.10.10.0/24), avec SVR-APPLI (Ubuntu, passerelle 10.10.10.1) et SVR-IMPRESSION (Windows 11, 10.10.10.13) comme machines de test.

## 2. Inventaire du serveur source

SVR-FICHIERS est un serveur de fichiers Windows 11 Pro en UEFI, avec 4 Go de RAM, un disque de 64 Go et une IP fixe 10.10.10.11.

L'inventaire a été réalisé avec `msinfo32`, `diskmgmt.msc`, `ipconfig /all`, `slmgr /dli` et `Get-SmbShare`.

| Élément | Valeur |
| --- | --- |
| Rôle | Serveur de fichiers (partage « Partage » → C:\Partage) |
| Système | Windows 11 Professionnel, build 26200 |
| Processeur | Intel Core 9 270H, 2 cœurs |
| RAM | 4 Go |
| Firmware | UEFI, démarrage sécurisé désactivé, TPM présent |
| Disque | 64 Go NVMe : EFI 200 Mo, C: 62,91 Go NTFS (≈ 31,4 Go utilisés), récupération 901 Mo |
| Carte réseau | Intel 82574L, MAC 00-0C-29-4F-8E-B0 |
| Configuration IP | 10.10.10.11/24 fixe, passerelle 10.10.10.1, DNS 1.1.1.1 |
| Réseau VMware | Segment LAN LABO-ISOLE |
| Licence | Windows Pro, canal Retail, non activée (erreur 0xC004F034) |
| Chiffrement | Chiffrement de l'appareil inactif (pas de BitLocker) |
| Droits du partage | Compte `employe` uniquement (avant correction) |

![image](https://hackmd.io/_uploads/rytyLMFoMl.png)

![image](https://hackmd.io/_uploads/SydlLzKjfx.png)


![image](https://hackmd.io/_uploads/r1AbIGtsMl.png)


![image](https://hackmd.io/_uploads/HJEN8Mtjfe.png)


![image](https://hackmd.io/_uploads/SkhSIGKsfx.png)


![image](https://hackmd.io/_uploads/ryeAIUMtsze.png)


La licence Windows n'était déjà pas activée avant la migration. Ce point est noté pour ne pas l'attribuer à tort à la migration.

## 3. Choix des outils

La migration a été réalisée avec Disk2vhd pour la capture et StarWind V2V Converter pour la conversion, deux outils gratuits.

| Outil | Rôle | Pourquoi ce choix |
| --- | --- | --- |
| Disk2vhd v2.02 (Microsoft Sysinternals) | Capture du disque à chaud en image VHDX | Gratuit, fourni par Microsoft, capture le système pendant qu'il tourne grâce aux clichés VSS |
| StarWind V2V Converter 9.0 | Conversion VHDX → VMDK | VMware Workstation ne lit pas le format VHDX |
| VMware Workstation | Hébergement de la VM cible | Hyperviseur déjà utilisé pour les autres projets |

Alternative écartée : VMware vCenter Converter Standalone, l'outil P2V historique de VMware. Il est devenu difficile à obtenir depuis le rachat de VMware par Broadcom.

Alternative possible : la reconstruction d'un serveur neuf en VM avec copie des données. Elle a été écartée car le but est de conserver le système à l'identique (configuration, comptes, partage).

## 4. Procédure de migration

La capture a duré 51 s, la conversion 28 s, et la VM migrée a démarré du premier coup.

1. **Préparation** : création d'un dossier `C:\soso\P2v` sur l'hôte, partagé avec SVR-FICHIERS par un dossier partagé VMware (toujours activé, lecture et écriture).

![image](https://hackmd.io/_uploads/r1HOIMFjMe.png)

2. **Accès au dossier en mode administrateur** : Disk2vhd ne voyait pas le chemin `\\vmware-host\Shared Folders`. Le dossier a été monté en lecteur Y: depuis un PowerShell administrateur avec `net use Y: "\\vmware-host\Shared Folders"`.

![image](https://hackmd.io/_uploads/HJ_YUfYjGx.png)


3. **Capture avec Disk2vhd** : options *Use Vhdx* et *Use Volume Shadow Copy* cochées, les 3 volumes sélectionnés (EFI, récupération, C:). Destination : `Y:\Disk2vhd\SVR-FICHIERS.vhdx`. Durée : 51 s. Fichier obtenu : environ 28,8 Go.

![image](https://hackmd.io/_uploads/SJYcLGFszg.png)


![image](https://hackmd.io/_uploads/rJ0jUftjze.png)


4. **Conversion avec StarWind V2V** : VHDX converti en VMDK au format *VMware Workstation growable image*. Durée : 28 s. Fichier obtenu : `SVR-FICHIERS.vmdk`, environ 27,8 Go.

![image](https://hackmd.io/_uploads/HkgpIGYoze.png)


5. **Arrêt du serveur source** : SVR-FICHIERS est éteint avant le démarrage de la VM cible, car les deux ont le même nom et la même IP.

6. **Création de la VM cible SVR-FICHIERS-P2V** : profil Windows 10 x64 (voir problème 4), firmware UEFI sans démarrage sécurisé, 2 cœurs, 4 Go de RAM, disque NVMe existant `SVR-FICHIERS.vmdk` (format conservé), carte réseau sur le segment LAN LABO-ISOLE.

![image](https://hackmd.io/_uploads/HyGRUfKiMx.png)


![image](https://hackmd.io/_uploads/SJz1wzKsfg.png)


7. **Premier démarrage** : message sur le lecteur CD (sata0:1) sans conséquence, puis écran de connexion en 28 s. Connexion avec le compte Admin.

8. **Vérifications** : nom d'hôte, configuration IP, partages, licence et périphériques, puis tests réseau depuis SVR-APPLI.

## 5. Comparatif avant / après migration

Le service rendu est identique après migration ; seules les performances baissent légèrement (copie environ 17 % plus lente, démarrage 7 s plus long).

| Test | Avant (SVR-FICHIERS) | Après (SVR-FICHIERS-P2V) |
| --- | --- | --- |
| Nom d'hôte | SVR-FICHIERS | SVR-FICHIERS |
| Carte réseau | Intel 82574L, Ethernet0 | Intel 82574L, Ethernet0 |
| Adresse MAC | 00-0C-29-4F-8E-B0 | 00-0C-29-4E-78-3B |
| IP / passerelle / DNS | 10.10.10.11 / 10.10.10.1 / 1.1.1.1 | identiques |
| Partage | Partage → C:\Partage | identique |
| Licence | Retail, non activée | identique |
| Temps de démarrage | 19 s | 28 s (1er) / 26 s (2e) |
| Ping depuis SVR-APPLI | 0 % de perte, 0,82 ms en moyenne | 0 % de perte, 1,47 ms en moyenne |
| Accès au partage (smbclient) | OK | OK |
| Copie de 500 Mo | 2,38 s (≈ 214 Mo/s) | 2,87 s (≈ 177 Mo/s) |

![image](https://hackmd.io/_uploads/SkVlPfKsGe.png)


![image](https://hackmd.io/_uploads/rkE-PGYjMl.png)

![image](https://hackmd.io/_uploads/S1QzwzFoMg.png)


L'IP fixe a été conservée car la VM cible utilise le même modèle de carte réseau émulée (Intel 82574L). Windows l'a reconnue comme la même carte malgré la nouvelle adresse MAC.

La baisse de performance s'explique par le disque au format dynamique (growable), qui grossit au fur et à mesure des écritures, et par la détection du nouveau matériel au premier démarrage. L'écart de ping (moins de 1 ms) n'a aucun impact pour les utilisateurs.

## 6. Problèmes rencontrés et solutions

Cinq problèmes ont été rencontrés : trois sur le serveur source pendant l'inventaire, deux liés à la migration elle-même.

| # | Problème | Cause | Solution |
| --- | --- | --- | --- |
| 1 | Partage inaccessible depuis le réseau (`TcpTestSucceeded : False` sur le port 445) | Pare-feu Windows bloquant le partage de fichiers | Profil réseau passé en privé et règle de pare-feu « SMB entrant 445 » créée avec `New-NetFirewallRule` |
| 2 | Erreur 86 « mot de passe incorrect » depuis SVR-IMPRESSION malgré un mot de passe correct | SVR-FICHIERS et SVR-IMPRESSION ont le même SID machine (S-1-5-21-3029412888-…), car elles sont issues d'un clonage. Windows 11 récent bloque alors l'authentification entre elles. | Tests réalisés depuis SVR-APPLI (Ubuntu, `smbclient`). En production, chaque clone doit être généralisé avec Sysprep. |
| 3 | `NT_STATUS_ACCESS_DENIED` sur le partage | Le partage n'autorisait que le compte `employe` | Droits donnés au compte Admin sur le partage (`Grant-SmbShareAccess`) et sur le dossier NTFS (`icacls`) |
| 4 | Disque migré refusé par la VM cible : « disque déchiffré, ne peut pas être utilisé avec une VM chiffrée » | Le profil Windows 11 de VMware impose un TPM virtuel, donc le chiffrement de la VM | VM cible créée avec le profil Windows 10 x64, sans TPM. Un Windows 11 déjà installé démarre sans TPM. |
| 5 | Périphériques fantômes dans le Gestionnaire de périphériques | La VM hérite de l'historique matériel de la source | Aucun impact. Ils peuvent être désinstallés pour garder un système propre. |

![image](https://hackmd.io/_uploads/HkY7wGFjGx.png)


![image](https://hackmd.io/_uploads/rJdNvzYjGg.png)


![image](https://hackmd.io/_uploads/rJ_HvGFjMl.png)


![image](https://hackmd.io/_uploads/SJbvvzYoMx.png)


![image](https://hackmd.io/_uploads/B1HODfKsMl.png)

![image](https://hackmd.io/_uploads/H1rtDftszx.png)


![image](https://hackmd.io/_uploads/Hy6VuMYsfl.png)



![image](https://hackmd.io/_uploads/rJfxdfFiMg.png)


Périphériques fantômes relevés après migration :

- **General UDisk USB Device** : clé USB branchée un jour sur la machine source ;
- **3 × Cliché instantané de volume générique** : clichés VSS créés par Disk2vhd pour les 3 volumes capturés ;
- **Module de plateforme sécurisée 2.0** : TPM de la source, absent de la VM cible.

![image](https://hackmd.io/_uploads/rkNawMtjfg.png)


Aucune carte réseau fantôme n'est apparue, car le modèle de carte émulée est le même avant et après.

Deux difficultés mineures : Disk2vhd refusait le chemin du dossier partagé (résolu avec un lecteur Y: monté en administrateur), et l'assistant VMware créait d'abord un disque vide au lieu d'utiliser le disque existant (résolu avec le problème 4).

## 7. Limites et risques d'une migration P2V

Le P2V conserve le système à l'identique, mais il transporte aussi ses défauts et ses contraintes matérielles et légales.

| Risque | Explication | Mesure à prendre |
| --- | --- | --- |
| Licences | Une licence Windows OEM est liée au matériel et se désactive après migration ; elle n'est pas transférable légalement. | Vérifier le type de licence (`slmgr /dli`) avant migration et prévoir une licence Retail ou volume. |
| Pilotes | Le matériel virtuel est différent ; d'anciens pilotes restent installés. Si le contrôleur de disque change, Windows peut ne pas démarrer (INACCESSIBLE_BOOT_DEVICE). | Garder le même type de contrôleur et de firmware (ici NVMe et UEFI), puis nettoyer les pilotes fantômes. |
| Réseau | Sur un vrai serveur physique, la carte réseau change de modèle et l'IP fixe est souvent perdue. | Noter la configuration IP avant migration et la vérifier juste après le premier démarrage. |
| Doublon sur le réseau | La VM a le même nom, la même IP et le même SID que la source. | Ne jamais faire tourner la source et la VM en même temps ; débrancher ou mettre hors service la source. |
| Sécurité (TPM) | Sans TPM virtuel, BitLocker et Windows Hello ne sont plus disponibles. | Chiffrer le disque migré pour pouvoir ajouter un TPM virtuel. |
| Performances | La VM partage les ressources de l'hôte ; un disque dynamique est plus lent. | Utiliser un disque préalloué et dimensionner l'hôte. |
| Données | Une image incomplète ou faite pendant une écriture peut perdre des données. | Utiliser les clichés VSS, vérifier la taille de l'image et garder la source intacte jusqu'à validation. |
| Défauts hérités | Les problèmes de la source (pare-feu, droits, SID cloné) sont migrés eux aussi. | Faire un inventaire complet et corriger avant de migrer, comme ici. |

## 8. Conclusion

La migration P2V de SVR-FICHIERS est réussie : la VM SVR-FICHIERS-P2V rend exactement le même service, avec le même nom, la même IP et le même partage.

La capture et la conversion ont pris moins de 2 minutes au total. La plus grande partie du travail a porté sur l'inventaire et la résolution des problèmes : pare-feu, SID cloné, droits du partage, TPM et chiffrement.

Ce projet montre qu'une migration P2V se prépare surtout en amont. Un inventaire complet, des mesures de référence et la vérification des licences, du firmware et du type de disque évitent la plupart des pannes. Les performances légèrement inférieures restent acceptables pour un serveur de fichiers et pourraient être améliorées avec un disque préalloué.