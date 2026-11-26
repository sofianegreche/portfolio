# Compte rendu — Projet 2 : Segmentation en VLAN d'un réseau d'entreprise

| | |
|---|---|
| **Cours** | Networking Essentials — Infrastructure informatique et réseau d'une petite entreprise |
| **Classe** | E1A |
| **Niveau** | Intermédiaire |
| **Type** | PPE (1 semaine) |
| **Outil** | GNS3 (switch Ethernet intégré, 4 VPCS) |
| **Auteur** | Negreche Sofia |
| **Date** | 12 octobre 2026 |

[TOC]

---

## 1. Contexte

Une entreprise veut séparer le trafic de son service comptabilité de celui du reste des employés, pour des raisons de sécurité. La solution retenue est de découper le réseau en deux VLAN sur le switch.

## 2. Objectifs pédagogiques

- Comprendre le principe des VLAN.
- Configurer au moins 2 VLAN sur un switch manageable.
- Vérifier l'étanchéité entre les VLAN.

## 3. Principe d'un VLAN

Un **VLAN** (Virtual LAN) permet de découper un switch physique en plusieurs réseaux logiques indépendants. Chaque VLAN forme un **domaine de diffusion séparé** : les postes d'un VLAN ne voient pas le trafic des autres VLAN, comme s'ils étaient branchés sur des switchs différents.

Un port en mode **access** appartient à un seul VLAN. Le poste qui y est branché fait automatiquement partie de ce VLAN, sans aucune configuration de son côté.

:::info
Le poste ne « sait » pas dans quel VLAN il se trouve : c'est le port du switch sur lequel il est branché qui le détermine.
:::

## 4. Matériel utilisé (dans GNS3)

| Équipement | Rôle | Quantité |
|---|---|---|
| Ethernet switch GNS3 (Switch1) | Switch manageable avec VLAN | 1 |
| VPCS (PC1 à PC4) | Postes de travail | 4 |

Aucun routeur n'est utilisé : le but est de prouver que le switch isole les VLAN.

---

## 5. Plan VLAN et adressage

| Poste | Service | VLAN | Port du switch | Adresse IP | Masque |
|---|---|---|---|---|---|
| PC1 | Comptabilité | 10 | e0 | 192.168.10.11 | 255.255.255.0 |
| PC2 | Comptabilité | 10 | e1 | 192.168.10.12 | 255.255.255.0 |
| PC3 | Employés | 20 | e2 | 192.168.10.13 | 255.255.255.0 |
| PC4 | Employés | 20 | e3 | 192.168.10.14 | 255.255.255.0 |

:::warning
**Choix volontaire :** tous les postes sont dans le **même sous-réseau** 192.168.10.0/24. Ainsi, si un poste de la comptabilité ne peut pas joindre un poste des employés, c'est **uniquement à cause du VLAN** et non de l'adressage IP. C'est la preuve d'étanchéité la plus claire.
:::

---

## 6. Configuration des VLAN (étapes 1 et 2)

Dans GNS3 : clic droit sur Switch1 → **Configure**. Pour chaque port, on sélectionne la ligne dans la liste, on indique le numéro de VLAN, le type **access**, puis **Add**.

| Port | VLAN | Type |
|---|---|---|
| 0 | 10 | access |
| 1 | 10 | access |
| 2 | 20 | access |
| 3 | 20 | access |
| 4 à 7 | 1 (par défaut) | access |

Les ports 4 à 7 ne sont pas utilisés et restent dans le VLAN 1 par défaut.

![image](https://hackmd.io/_uploads/BycPxDcsMl.png)


### Vérification du câblage

Les noms d'interfaces ont été affichés dans GNS3 (« Show interface labels ») pour vérifier que chaque poste est bien branché sur le port prévu :

| Poste | Port du switch | VLAN obtenu |
|---|---|---|
| PC1 | e0 | 10 – Comptabilité |
| PC2 | e1 | 10 – Comptabilité |
| PC3 | e2 | 20 – Employés |
| PC4 | e3 | 20 – Employés |

![image](https://hackmd.io/_uploads/H1adxPcoGg.png)

:::info
Sur un vrai switch Cisco, la commande `show vlan brief` affiche directement la liste des VLAN et leurs ports. Le switch intégré de GNS3 n'a pas de console : la vérification se fait avec la fenêtre de configuration et les labels d'interfaces.
:::

---

## 7. Configuration des postes

Chaque VPCS est configuré avec la commande `ip`, puis la configuration est sauvegardée avec `save` :

```
PC1> ip 192.168.10.11 255.255.255.0 192.168.10.1
PC1> save
```

La passerelle 192.168.10.1 est renseignée pour un futur routeur, mais elle n'est pas utilisée dans ce projet puisque les tests restent dans le même sous-réseau.

![image](https://hackmd.io/_uploads/B119eP9jGg.png)


![image](https://hackmd.io/_uploads/SJWoeP5oGg.png)


![image](https://hackmd.io/_uploads/H1m3lDqjfx.png)


![image](https://hackmd.io/_uploads/r1f6lD9jzl.png)


![image](https://hackmd.io/_uploads/ryVAgv5jMg.png)


---

## 8. Tests d'étanchéité (étape 3)

| Test | VLAN source → destination | Résultat attendu | Résultat obtenu |
|---|---|---|---|
| PC1 → PC2 | 10 → 10 | Réponses | ✅ 5/5 réponses, TTL 64 |
| PC3 → PC4 | 20 → 20 | Réponses | ✅ 5/5 réponses, TTL 64 |
| PC1 → PC3 | 10 → 20 | Bloqué | ❌ host not reachable |
| PC2 → PC4 | 10 → 20 | Bloqué | ❌ host not reachable |

### 8.1 Communication dans un même VLAN

![image](https://hackmd.io/_uploads/r1IbWw5iGe.png)


![image](https://hackmd.io/_uploads/HyogMPcjfg.png)


:::success
Les postes d'un même VLAN communiquent normalement, avec des temps de réponse inférieurs à 1,5 ms : le réseau fonctionne.
:::

### 8.2 Communication entre VLAN différents

![image](https://hackmd.io/_uploads/B1DQ-wqsMx.png)


![image](https://hackmd.io/_uploads/Sk8PWwciMg.png)


:::danger
Un poste de la comptabilité ne peut pas joindre un poste des employés, alors qu'ils sont dans le même sous-réseau IP : **les VLAN sont bien étanches**.
:::

### 8.3 Explication

Pour joindre une machine, un poste envoie d'abord une **requête ARP** en broadcast (« qui possède l'adresse 192.168.10.13 ? »). Le switch ne diffuse ce broadcast **que sur les ports du même VLAN**. La requête de PC1 (VLAN 10) n'arrive donc jamais à PC3 (VLAN 20), personne ne répond, et le VPCS affiche « host not reachable ».

---

## 9. Difficultés rencontrées et solutions

| Problème | Cause | Solution |
|---|---|---|
| Champs « Port » et « VLAN » inversés dans la configuration du switch | Confusion entre le numéro de port (prise du switch) et le numéro de VLAN | Port = numéro de la prise (0, 1, 2…), VLAN = 10 ou 20 |
| Commande refusée sur PC1 et PC4 (« Bad command ») | Adresse tapée sans le mot-clé `ip` devant | Taper `ip 192.168.10.xx 255.255.255.0 192.168.10.1` |
| Configuration de PC2 non sauvegardée | Oubli de `save` après la modification | Relancer `save` |
| Savoir dans quel VLAN se trouve un poste | Le VPCS n'affiche pas son VLAN | Croiser la configuration des ports du switch avec les labels d'interfaces |

![image](https://hackmd.io/_uploads/S1TDfwqjfx.png)


![image](https://hackmd.io/_uploads/r1COfDcjzx.png)


---

## 10. Conclusion

Les deux VLAN sont configurés et opérationnels : le VLAN 10 regroupe la comptabilité (PC1, PC2) et le VLAN 20 les employés (PC3, PC4). Les tests prouvent que les postes d'un même VLAN communiquent, tandis que le trafic entre la comptabilité et les employés est bloqué, même avec des adresses IP dans le même sous-réseau. L'objectif de sécurité est atteint.

Ce projet m'a permis de comprendre qu'un VLAN sépare les domaines de diffusion au niveau du switch (couche 2), indépendamment de l'adressage IP. Pour permettre plus tard une communication contrôlée entre les deux services, il faudrait ajouter un routeur (routage inter-VLAN) avec des règles de filtrage.

---

*Negreche Sofia — E1A*