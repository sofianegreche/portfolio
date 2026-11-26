# Compte rendu — Projet 1 : Câblage et adressage IP d'un petit bureau

| | |
|---|---|
| **Cours** | Networking Essentials — Infrastructure informatique et réseau d'une petite entreprise |
| **Classe** | E1A |
| **Type** | PPE (1 semaine) |
| **Outil** | GNS3 (routeur Cisco, switch Ethernet GNS3, 8 VPCS) |
| **Auteur** | Negreche Sofia |
| **Date** | 12 octobre 2026 |

[TOC]

---

## 1. Contexte

Un bureau de 8 postes doit être câblé et adressé en IP statique sur un même sous-réseau. Le réseau a été réalisé et testé dans le simulateur GNS3.

## 2. Objectifs pédagogiques

- Concevoir un plan d'adressage IP cohérent.
- Câbler et connecter switch, routeur et postes.
- Vérifier la connectivité de bout en bout.

## 3. Matériel utilisé (dans GNS3)

| Équipement | Rôle | Quantité |
|---|---|---|
| Routeur Cisco (R1) | Passerelle par défaut du réseau | 1 |
| Switch Ethernet GNS3 (Switch1) | Relie tous les équipements dans le même réseau | 1 |
| VPCS (PC1 à PC8) | Postes de travail | 8 |

---

## 4. Plan d'adressage (étape 1)

| Élément | Valeur |
|---|---|
| Adresse réseau | 192.168.10.0 |
| Masque | 255.255.255.0 (/24) |
| Passerelle | 192.168.10.1 (R1, interface G0/0) |
| Plage des postes | 192.168.10.11 à 192.168.10.18 |
| Adresse de broadcast | 192.168.10.255 |
| Nombre d'hôtes possibles | 254 |

:::info
Le masque /24 laisse largement assez d'adresses pour les 8 postes et permet d'ajouter d'autres équipements plus tard (imprimante, serveur, nouveaux postes).
:::

### Tableau d'adressage détaillé

| Équipement | Interface | Adresse IP | Masque | Passerelle | Port du switch |
|---|---|---|---|---|---|
| R1 | GigabitEthernet0/0 | 192.168.10.1 | 255.255.255.0 | – | 8 |
| PC1 | Ethernet0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 | 0 |
| PC2 | Ethernet0 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 | 1 |
| PC3 | Ethernet0 | 192.168.10.13 | 255.255.255.0 | 192.168.10.1 | 2 |
| PC4 | Ethernet0 | 192.168.10.14 | 255.255.255.0 | 192.168.10.1 | 3 |
| PC5 | Ethernet0 | 192.168.10.15 | 255.255.255.0 | 192.168.10.1 | 4 |
| PC6 | Ethernet0 | 192.168.10.16 | 255.255.255.0 | 192.168.10.1 | 5 |
| PC7 | Ethernet0 | 192.168.10.17 | 255.255.255.0 | 192.168.10.1 | 6 |
| PC8 | Ethernet0 | 192.168.10.18 | 255.255.255.0 | 192.168.10.1 | 7 |

---

## 5. Câblage (étape 2)

La topologie choisie est une **topologie en étoile** : tous les postes et le routeur sont reliés à un switch central.

Le switch Ethernet de GNS3 possède 8 ports par défaut (0 à 7). Il fallait 9 ports (8 postes + le routeur), donc un **port 8** a été ajouté dans la configuration du switch (VLAN 1, mode access). Tous les ports sont dans le VLAN 1 : les postes et la passerelle sont donc dans le même réseau.

| Équipement | Interface | Relié à |
|---|---|---|
| R1 | GigabitEthernet0/0 | Switch1, port 8 |
| PC1 à PC8 | Ethernet0 | Switch1, ports 0 à 7 |

![image](https://hackmd.io/_uploads/S1Ilaf9jfx.png)


![image](https://hackmd.io/_uploads/r1MAxXqjMg.png)


---

## 6. Configuration

### 6.1 Routeur R1

Au départ, toutes les interfaces du routeur sont sans adresse IP et désactivées (« administratively down »), ce qui est normal pour un routeur Cisco neuf.

Commandes utilisées :

```
R1# configure terminal
R1(config)# interface g0/0
R1(config-if)# ip address 192.168.10.1 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# do write memory
```

- `ip address` : attribue l'adresse IP et le masque à l'interface.
- `no shutdown` : active l'interface (elle passe à « up »).
- `write memory` : sauvegarde la configuration dans la NVRAM pour qu'elle ne soit pas perdue au redémarrage.

![image](https://hackmd.io/_uploads/SysHaMcjze.png)

![image](https://hackmd.io/_uploads/SklwaGqifl.png)


Vérification avec `show ip interface brief` :

```
Interface            IP-Address     OK? Method Status   Protocol
GigabitEthernet0/0   192.168.10.1   YES manual up       up
```

:::success
L'interface G0/0 est configurée manuellement en 192.168.10.1 et active (statut up, protocole up). Elle joue le rôle de passerelle par défaut pour les 8 postes.
:::

![image](https://hackmd.io/_uploads/HyW_6M9izg.png)

### 6.2 Postes PC1 à PC8 (étape 3)

Les VPCS de GNS3 n'utilisent pas les commandes Cisco : il n'y a pas de mode configuration. L'adressage se fait directement avec la commande `ip`.

Exemple pour PC1 :

```
PC1> ip 192.168.10.11 255.255.255.0 192.168.10.1
PC1> save
PC1> show ip
```

- `ip <adresse> <masque> <passerelle>` : configure l'adresse IP statique.
- `save` : enregistre la configuration dans le fichier `startup.vpc`.
- `show ip` : affiche la configuration pour vérification.

![image](https://hackmd.io/_uploads/BJ3Y6z5ozx.png)

La même configuration a été appliquée sur PC2 à PC8 en changeant seulement le dernier octet (.12 à .18). À chaque fois, le VPCS a vérifié qu'il n'y avait pas d'adresse en double (« Checking for duplicate address »).

:::info
Après la commande `ip`, chaque VPCS affiche « PC1 : 192.168.10.xx ». C'est le nom interne de l'instance VPCS, cela n'a pas de lien avec le nom du poste dans GNS3.
:::

![image](https://hackmd.io/_uploads/Sk-jpM9ofg.png)


![image](https://hackmd.io/_uploads/HkghTM9jMe.png)


![image](https://hackmd.io/_uploads/H1Ppazcize.png)

![image](https://hackmd.io/_uploads/HJrQ-m5ofx.png)


![image](https://hackmd.io/_uploads/Hyel0fqjMx.png)

![image](https://hackmd.io/_uploads/ryqDb79jfe.png)


![image](https://hackmd.io/_uploads/Hygz0Mcsfx.png)


---

## 7. Tests de connectivité (étape 4)

### 7.1 Poste → passerelle

| Source | Destination | Résultat | TTL |
|---|---|---|---|
| PC1 (192.168.10.11) | R1 (192.168.10.1) | 5/5 réponses, 0 % de perte | 255 |
| PC8 (192.168.10.18) | R1 (192.168.10.1) | 5/5 réponses, 0 % de perte | 255 |

Le TTL de 255 est la valeur par défaut d'un routeur Cisco : c'est bien R1 qui répond. Le premier ping est un peu plus long (environ 50 à 90 ms) à cause de la résolution ARP (le poste doit d'abord trouver l'adresse MAC du routeur).

![image](https://hackmd.io/_uploads/Bk1NCMqsGl.png)


![image](https://hackmd.io/_uploads/BkcHCMcoGx.png)


### 7.2 Poste → poste

Depuis PC8, un ping a été envoyé vers chacun des 7 autres postes :

| Destination | Résultat | TTL | Temps moyen |
|---|---|---|---|
| PC1 (192.168.10.11) | 2/2 réponses | 64 | ≈ 1,9 ms |
| PC2 (192.168.10.12) | 2/2 réponses | 64 | ≈ 1,5 ms |
| PC3 (192.168.10.13) | 2/2 réponses | 64 | ≈ 1,4 ms |
| PC4 (192.168.10.14) | 2/2 réponses | 64 | ≈ 2,0 ms |
| PC5 (192.168.10.15) | 2/2 réponses | 64 | ≈ 1,6 ms |
| PC6 (192.168.10.16) | 2/2 réponses | 64 | ≈ 1,5 ms |
| PC7 (192.168.10.17) | 2/2 réponses | 64 | ≈ 1,5 ms |

:::success
Tous les postes se joignent, sans aucune perte. Le TTL de 64 (valeur par défaut des VPCS) montre que la réponse vient directement d'un autre poste du même réseau, sans passer par le routeur. Les temps très faibles (1 à 2 ms) confirment que le trafic reste local, à travers le switch.
:::

![image](https://hackmd.io/_uploads/r14vCMcofl.png)

---

## 8. Schéma réseau (étape 5)

```
                      PC1      PC2      PC3      PC4
                     .11      .12      .13      .14
                       \        |        |        /
                     [ p0 ]  [ p1 ]   [ p2 ]  [ p3 ]
                    +-----------------------------------+
                    |              Switch1              | [ p8 ] ---- G0/0  R1
                    +-----------------------------------+             192.168.10.1
                     [ p4 ]  [ p5 ]   [ p6 ]  [ p7 ]
                       /        |        |        \
                     .15      .16      .17      .18
                      PC5      PC6      PC7      PC8

                        Réseau : 192.168.10.0 /24
```

![image](https://hackmd.io/_uploads/BJRBxQciGx.png)

---

## 9. Difficultés rencontrées et solutions

| Problème | Cause | Solution |
|---|---|---|
| `ip adresse ...` refusé (« Invalid input ») | Les commandes Cisco IOS sont en anglais | Utiliser `ip address` |
| Masque 255.255.255.1 refusé (« Bad mask ») | Un masque doit être une suite continue de 1 puis de 0 en binaire | Utiliser 255.255.255.0 |
| `do show ip interface brief` refusé | `do` ne fonctionne qu'en mode configuration | En mode privilégié (`R1#`), taper directement `show ip interface brief` |
| `conf t` refusé sur les VPCS | Les VPCS ne sont pas des équipements Cisco | Configurer directement avec la commande `ip` |
| Premier ping PC1 → R1 échoué (« host not reachable ») | Les postes étaient branchés directement sur le routeur, chacun sur une interface différente | Ajout d'un switch central : les 8 postes et R1 (G0/0) sont branchés dessus |
| Switch avec seulement 8 ports | Le switch GNS3 a 8 ports par défaut | Ajout d'un 9e port (port 8) pour le routeur |

:::warning
**Pourquoi le premier ping a échoué :** chaque interface d'un routeur est un réseau séparé. Les postes branchés directement sur R1 n'étaient donc pas dans le même réseau que G0/0. C'est le switch qui permet de regrouper les 8 postes et la passerelle dans un même sous-réseau.
:::

![image](https://hackmd.io/_uploads/B1bvx79izl.png)


![image](https://hackmd.io/_uploads/SJ1ueQciGg.png)


![image](https://hackmd.io/_uploads/BJ3dgmqiMl.png)

---

## 10. Conclusion

Le bureau de 8 postes est entièrement fonctionnel : tous les postes sont dans le réseau 192.168.10.0/24, se joignent entre eux et joignent la passerelle R1. Les trois objectifs du projet sont atteints : plan d'adressage cohérent, câblage en étoile autour d'un switch, et connectivité vérifiée de bout en bout par des pings.

Ce projet m'a permis de comprendre le rôle du switch (relier les équipements d'un même réseau) et celui du routeur (passerelle vers d'autres réseaux), ainsi que la différence entre les commandes Cisco IOS et celles des VPCS. Le dépannage du premier ping échoué a aussi montré l'importance de bien vérifier la topologie avant l'adressage.

---

Negreche Sofia — E1A*