# Projet 4 – Dépannage et diagnostic d’une panne réseau

## Contexte

Dans le cadre de ce projet, j’ai réalisé une intervention de dépannage sur une **machine virtuelle**.

L’objectif était de simuler une panne réseau, d’identifier son origine à l’aide d’une démarche de diagnostic structurée, puis de rétablir la connexion.

La panne a été volontairement créée en modifiant la configuration réseau de la machine virtuelle.

> **Particularité du scénario :** le dépannage a été réalisé sur une machine virtuelle. La panne concernait donc la configuration de l’adaptateur réseau virtuel et non un câble réseau ou une carte réseau physique.

---

# 1. Symptôme constaté

Après avoir modifié la configuration réseau de la machine virtuelle, la connexion réseau ne fonctionnait plus correctement.

Le symptôme principal était :

* absence d’accès au réseau depuis la machine virtuelle ;
* impossibilité d’utiliser normalement Internet ;
* la machine virtuelle démarrait correctement, mais la connexion réseau ne fonctionnait pas.

Le problème semblait donc être lié à la configuration réseau de la machine virtuelle.

---

# 2. Création de la panne

Afin de réaliser le diagnostic, j’ai volontairement provoqué une panne réseau.

Dans les paramètres de la machine virtuelle, j’ai ouvert la configuration de l’adaptateur réseau et modifié son mode de connexion.

### Configuration utilisée

* **Type de machine :** machine virtuelle
* **Adaptateur réseau :** activé
* **Mode réseau :** NAT

Cette modification a permis de simuler une panne réseau afin de pouvoir appliquer une procédure de diagnostic et de dépannage.

---

# 3. Méthodologie de diagnostic

Pour identifier l’origine du problème, j’ai suivi une démarche structurée :

**Identifier → Isoler → Tester → Résoudre → Vérifier**

L’objectif était de déterminer précisément l’origine de la panne avant de modifier la configuration.

---

# 4. Identification du problème

Le premier constat était que la machine virtuelle démarrait normalement, mais que la connexion réseau ne fonctionnait pas.

J’ai donc vérifié les différents éléments pouvant être à l’origine du problème.

Les premiers constats étaient :

* le système d’exploitation démarrait correctement ;
* la machine virtuelle fonctionnait normalement ;
* le problème semblait uniquement concerner la connexion réseau ;
* l’adaptateur réseau virtuel était activé.

Le problème ne semblait donc pas être lié au démarrage ou au fonctionnement général du système.

---

# 5. Hypothèses testées

Plusieurs hypothèses ont été envisagées afin d’identifier la cause de la panne.

| Hypothèse                                             | Vérification                                           | Résultat                 |
| ----------------------------------------------------- | ------------------------------------------------------ | ------------------------ |
| Le système d’exploitation rencontre un problème       | Vérification du démarrage et du fonctionnement général | Hypothèse écartée        |
| L’adaptateur réseau virtuel est désactivé             | Vérification des paramètres réseau de la VM            | Hypothèse écartée        |
| Le problème vient de la configuration réseau de la VM | Vérification du mode de connexion réseau               | Hypothèse retenue        |
| Le problème vient d’un périphérique physique          | Vérification du contexte de la machine                 | Hypothèse non applicable |

---

# 6. Diagnostic

Après les différentes vérifications, j’ai identifié que le problème était lié à la **configuration du réseau de la machine virtuelle**.

L’adaptateur réseau était configuré en mode **NAT**.

Dans le contexte de mon test, cette configuration ne permettait pas à la machine virtuelle d’accéder correctement au réseau.

Le diagnostic retenu est donc :

> **La panne était due à une configuration réseau inadaptée de la machine virtuelle.**

---

# 7. Résolution

Pour résoudre le problème, j’ai modifié la configuration de l’adaptateur réseau de la machine virtuelle.

### Étapes effectuées

1. J’ai ouvert les paramètres de la machine virtuelle.
2. Je suis allé dans la section **Network / Réseau**.
3. J’ai sélectionné l’adaptateur réseau.
4. J’ai vérifié le mode de connexion utilisé.
5. Le mode était configuré sur **NAT**.
6. J’ai remplacé **NAT** par **Bridged Adapter / Bridge**.
7. J’ai validé la nouvelle configuration.
8. J’ai redémarré la machine virtuelle afin d’appliquer correctement la modification.

Après cette modification, la connexion réseau a de nouveau fonctionné.

---

# 8. Vérification après dépannage

Une fois la configuration modifiée, j’ai effectué plusieurs tests afin de vérifier que le problème était bien résolu.

J’ai notamment vérifié :

* que l’adaptateur réseau était activé ;
* que la connexion réseau était disponible ;
* que la machine virtuelle pouvait accéder à Internet ;
* que les pages Web pouvaient être chargées ;
* que le fonctionnement général de la machine virtuelle était normal.

### Résultat

La connexion réseau fonctionnait à nouveau après le passage du mode **NAT** au mode **Bridge**.

La panne a donc été résolue.

---

# 9. Rapport d’intervention / Ticket

| Élément                    | Informations                             |
| -------------------------- | ---------------------------------------- |
| **Type d’intervention**    | Dépannage réseau                         |
| **Équipement**             | Machine virtuelle                        |
| **Problème**               | Absence de connexion réseau              |
| **Symptôme**               | Internet inaccessible depuis la VM       |
| **Cause identifiée**       | Configuration du mode réseau virtuel     |
| **Configuration initiale** | NAT                                      |
| **Configuration corrigée** | Bridge                                   |
| **Action réalisée**        | Modification du mode de connexion réseau |
| **Résultat**               | Connexion rétablie                       |
| **Statut**                 | Résolu                                   |

---

# 10. Démarche de raisonnement

La démarche suivie pendant le dépannage a été la suivante.

### Identifier

J’ai constaté que la machine virtuelle démarrait correctement, mais que la connexion réseau ne fonctionnait pas.

### Isoler

J’ai déterminé que le problème semblait limité à la partie réseau et non au fonctionnement général de la machine virtuelle.

### Tester

J’ai vérifié les paramètres de l’adaptateur réseau virtuel et identifié que le mode de connexion était configuré en **NAT**.

### Diagnostiquer

Après vérification de la configuration, j’ai conclu que le problème venait du mode de connexion réseau utilisé par la machine virtuelle.

### Résoudre

J’ai remplacé le mode **NAT** par le mode **Bridge** dans les paramètres réseau de la machine virtuelle.

### Vérifier

Après la modification, j’ai testé la connexion Internet et constaté que le réseau fonctionnait à nouveau correctement.

---

# 11. Recommandations

Pour éviter ce type de problème à l’avenir, il est recommandé de :

* vérifier le mode réseau configuré pour la machine virtuelle ;
* vérifier que l’adaptateur réseau virtuel est activé ;
* tester la connexion après toute modification de la configuration réseau ;
* documenter les modifications effectuées ;
* revenir à la dernière configuration fonctionnelle connue en cas de problème.

Dans une machine virtuelle, il faut également distinguer un problème provenant du **système d’exploitation invité** d’un problème provenant de la **configuration réseau de la machine virtuelle**.

---

# 12. Conclusion

Cette intervention m’a permis de mettre en pratique une méthodologie de dépannage structurée sur une machine virtuelle.

J’ai volontairement créé une panne réseau en utilisant une configuration **NAT**. Après avoir analysé le problème et vérifié les différentes hypothèses, j’ai identifié la configuration réseau de la machine virtuelle comme étant à l’origine de la panne.

Pour résoudre le problème, j’ai modifié le mode de connexion de **NAT** vers **Bridge**.

Après cette modification, la connexion réseau a été rétablie et différents tests ont confirmé le bon fonctionnement de la machine virtuelle.

La démarche suivie peut être résumée ainsi :

> **Identifier → Isoler → Tester → Diagnostiquer → Résoudre → Vérifier**

Le dépannage est terminé et la machine virtuelle fonctionne normalement.
