# Projet 5 — Conception réseau complète pour une PME multi-sites

| | |
|---|---|
| **Cours** | Networking Essentials — Infrastructure informatique et réseau d'une petite entreprise |
| **Classe** | E1A |
| **Niveau** | Avancé |
| **Durée** | 1 semaine (PPE) |
| **Auteur** | Negreche Sofia |
| **Date** | 16.10.2026 |
| **Outils** | GNS3 — routeurs Cisco c7200, Ethernet switch GNS3, VPCS |

---

## 1. Contexte

Une PME possède deux sites distants : le **siège à Genève** et une **agence à Lausanne**. Les deux sites doivent pouvoir communiquer de façon **sécurisée**, notamment pour que l'agence accède au serveur de fichiers du siège.

L'objectif est de concevoir l'architecture réseau cible (adressage, VLAN, interconnexion), de la valider sur une maquette GNS3 et de justifier les choix techniques.

---

## 2. Recueil des besoins

| | Genève (siège) | Lausanne (agence) |
|---|---|---|
| Postes Administration (direction, compta) | 6 | 3 |
| Postes Employés | 14 | 7 |
| Serveurs | 1 serveur de fichiers | — |
| Besoin principal | Héberger les services partagés | Accéder au serveur de fichiers de Genève |

**Contraintes :**
- Les échanges entre les sites doivent être **chiffrés** (ils passent par Internet).
- L'administration doit être **séparée** des employés (segmentation par VLAN).
- Le plan d'adressage doit pouvoir **accueillir un 3e site** plus tard.

---

## 3. Schéma d'architecture

![image](https://hackmd.io/_uploads/BJ9sBj1hMg.png)

**Équipements :**

| Équipement | Rôle |
|---|---|
| R-GENEVE | Routeur du siège : routage inter-VLAN + extrémité VPN |
| R-LAUSANNE | Routeur de l'agence : routage inter-VLAN + extrémité VPN |
| R-ISP | Simule Internet (opérateur) entre les deux sites |
| SW-GENEVE / SW-LAUSANNE | Switchs d'accès avec VLAN, trunk 802.1Q vers le routeur |
| PC-GENEVE, PC-EMP-GE, PC-LAUSANNE, PC-EMP-LSN | Postes clients (VPCS) |
| SVR-FICHIERS | Serveur de fichiers partagé (VPCS) |

---

## 4. Plan d'adressage

### 4.1 Règle d'adressage

Format : **10.site.VLAN.0/24**
- 2e octet = numéro du site (1 = Genève, 2 = Lausanne)
- 3e octet = numéro du VLAN

### 4.2 Tableau d'adressage

| Site | VLAN | Nom | Réseau | Passerelle | Hôte |
|---|---|---|---|---|---|
| Genève | 10 | ADMIN | 10.1.10.0/24 | 10.1.10.1 | PC-GENEVE — 10.1.10.11 |
| Genève | 20 | EMPLOYES | 10.1.20.0/24 | 10.1.20.1 | PC-EMP-GE — 10.1.20.11 |
| Genève | 99 | SERVEURS | 10.1.99.0/24 | 10.1.99.1 | SVR-FICHIERS — 10.1.99.10 |
| Lausanne | 10 | ADMIN | 10.2.10.0/24 | 10.2.10.1 | PC-LAUSANNE — 10.2.10.11 |
| Lausanne | 20 | EMPLOYES | 10.2.20.0/24 | 10.2.20.1 | PC-EMP-LSN — 10.2.20.11 |

### 4.3 Liens WAN (IP publiques simulées)

| Lien | Réseau | Adresses |
|---|---|---|
| Genève ↔ R-ISP | 203.0.113.0/30 | R-GENEVE Gi1/0 = .1 — R-ISP Gi0/0 = .2 |
| Lausanne ↔ R-ISP | 198.51.100.0/30 | R-LAUSANNE Gi1/0 = .1 — R-ISP Gi1/0 = .2 |

### 4.4 Justification

- **Un /24 par VLAN** : 254 adresses utilisables, donc beaucoup de marge par rapport aux besoins (max. 14 postes).
- **Un /16 par site** (10.1.0.0/16 et 10.2.0.0/16) : une seule ligne d'ACL suffit pour définir le trafic à chiffrer dans le VPN.
- **Évolutif** : un 3e site prendrait simplement 10.3.0.0/16, sans rien modifier sur les sites existants.
- **Lisible** : en voyant 10.2.20.11, on sait directement qu'il s'agit du site de Lausanne, VLAN Employés.
- **/30 sur les liens WAN** : seulement 2 adresses utiles, ce qui suffit pour un lien point à point. Les plages 203.0.113.0 et 198.51.100.0 sont réservées à la documentation et servent à simuler des IP publiques.

---

## 5. VLAN et routage inter-VLAN

| VLAN | Nom | Genève | Lausanne |
|---|---|---|---|
| 10 | ADMIN | ✅ | ✅ |
| 20 | EMPLOYES | ✅ | ✅ |
| 99 | SERVEURS | ✅ | — |

**Configuration des switchs :**
- Port **e0** → **dot1q** (trunk 802.1Q vers le routeur)
- Ports **e1 / e2 / e3** → **access** VLAN 10 / 20 / 99

![image](https://hackmd.io/_uploads/Bk4Z8syhzl.png)


![image](https://hackmd.io/_uploads/SkvX8iJnzg.png)

**Routage inter-VLAN** : méthode **router-on-a-stick**. Le routeur utilise une seule interface physique (Gi0/0) découpée en sous-interfaces, une par VLAN (Gi0/0.10, Gi0/0.20, Gi0/0.99).

**Justification** : les VLAN séparent l'administration, les employés et les serveurs. Ça limite la propagation des broadcasts, améliore la sécurité et permet plus tard de filtrer les accès entre VLAN avec des ACL. Le router-on-a-stick évite d'acheter un switch de niveau 3 pour une petite structure.

---

## 6. Interconnexion des sites

### 6.1 Comparaison des solutions

| Critère | VPN IPsec site-à-site | Ligne dédiée / MPLS |
|---|---|---|
| Coût | Faible (utilise l'accès Internet existant) | Élevé (abonnement mensuel opérateur) |
| Sécurité | Chiffrement AES-256 | Réseau privé de l'opérateur, pas chiffré par défaut |
| Débit / latence | Dépend d'Internet, pas garanti | Garanti par contrat (SLA) |
| Mise en place | Rapide, configuration sur les routeurs | Plusieurs semaines (installation opérateur) |
| Adapté à une PME | ✅ Oui | Plutôt pour les grandes entreprises |

### 6.2 Choix retenu : VPN IPsec site-à-site

Pour une PME avec deux sites et des besoins modestes, le VPN IPsec offre le meilleur rapport coût / sécurité. Les données sont chiffrées de bout en bout entre les deux routeurs, même si elles traversent Internet.

### 6.3 Paramètres du VPN

| Paramètre | Valeur | Rôle |
|---|---|---|
| Phase 1 (IKE) — chiffrement | AES 256 | Protège la négociation |
| Phase 1 — intégrité | SHA | Vérifie que les messages ne sont pas modifiés |
| Phase 1 — authentification | Clé pré-partagée (PSK) | Les deux routeurs se reconnaissent |
| Phase 1 — Diffie-Hellman | Groupe 14 (2048 bits) | Échange de clés sécurisé |
| Phase 1 — durée de vie | 86 400 s (24 h) | Renouvellement des clés |
| Phase 2 (IPsec) | ESP AES-256 + SHA-HMAC, mode tunnel | Chiffre les données |
| Trafic protégé (ACL) | 10.1.0.0/16 ↔ 10.2.0.0/16 | Seul le trafic entre les sites est chiffré |

---

## 7. Mise en œuvre

### 7.1 R-ISP (Internet simulé)

```
hostname R-ISP
interface Gi0/0
 description WAN - vers R-GENEVE
 ip address 203.0.113.2 255.255.255.252
 no shutdown
interface Gi1/0
 description WAN - vers R-LAUSANNE
 ip address 198.51.100.2 255.255.255.252
 no shutdown
```

![image](https://hackmd.io/_uploads/SJ8H8o13Mg.png)


> R-ISP ne connaît **aucune route** vers les réseaux privés 10.x.x.x, comme un vrai routeur Internet.

### 7.2 R-GENEVE — interfaces et routage

```
interface Gi0/0
 description LAN - Trunk vers SW-GENEVE
 no shutdown
interface Gi0/0.10
 encapsulation dot1Q 10
 ip address 10.1.10.1 255.255.255.0
interface Gi0/0.20
 encapsulation dot1Q 20
 ip address 10.1.20.1 255.255.255.0
interface Gi0/0.99
 encapsulation dot1Q 99
 ip address 10.1.99.1 255.255.255.0
interface Gi1/0
 description WAN - Internet
 ip address 203.0.113.1 255.255.255.252
 no shutdown
ip route 0.0.0.0 0.0.0.0 203.0.113.2
```

### 7.3 R-LAUSANNE — interfaces et routage

```
interface Gi0/0
 description LAN - Trunk vers SW-LAUSANNE
 no shutdown
interface Gi0/0.10
 encapsulation dot1Q 10
 ip address 10.2.10.1 255.255.255.0
interface Gi0/0.20
 encapsulation dot1Q 20
 ip address 10.2.20.1 255.255.255.0
interface Gi1/0
 description WAN - Internet
 ip address 198.51.100.1 255.255.255.252
 no shutdown
ip route 0.0.0.0 0.0.0.0 198.51.100.2
```

![image](https://hackmd.io/_uploads/BJs38jk2Gl.png)



![image](https://hackmd.io/_uploads/SynoIok3Gx.png)


### 7.4 Configuration des postes (VPCS)

| VPCS | Commande |
|---|---|
| PC-GENEVE | `ip 10.1.10.11/24 10.1.10.1` |
| PC-EMP-GE | `ip 10.1.20.11/24 10.1.20.1` |
| SVR-FICHIERS | `ip 10.1.99.10/24 10.1.99.1` |
| PC-LAUSANNE | `ip 10.2.10.11/24 10.2.10.1` |
| PC-EMP-LSN | `ip 10.2.20.11/24 10.2.20.1` |

Puis `save` sur chaque VPCS.

![image](https://hackmd.io/_uploads/SJnC8s1nGe.png)
![image](https://hackmd.io/_uploads/BJ7Dui1nMl.png)
![image](https://hackmd.io/_uploads/S1lOdi13Ml.png)
![image](https://hackmd.io/_uploads/S1MF_o12Gx.png)
![image](https://hackmd.io/_uploads/BJeqdskhGg.png)



### 7.5 VPN IPsec — R-GENEVE

```
crypto isakmp policy 10
 encryption aes 256
 hash sha
 authentication pre-share
 group 14
 lifetime 86400
crypto isakmp key VPN-PME2026 address 198.51.100.1
crypto ipsec transform-set TS-GE-LS esp-aes 256 esp-sha-hmac
 mode tunnel
ip access-list extended VPN-TRAFIC
 permit ip 10.1.0.0 0.0.255.255 10.2.0.0 0.0.255.255
crypto map CMAP-VPN 10 ipsec-isakmp
 set peer 198.51.100.1
 set transform-set TS-GE-LS
 match address VPN-TRAFIC
interface Gi1/0
 crypto map CMAP-VPN
```

![image](https://hackmd.io/_uploads/S1eLDoknfe.png)


### 7.6 VPN IPsec — R-LAUSANNE

Configuration miroir : peer = **203.0.113.1** et ACL inversée.

```
crypto isakmp policy 10
 encryption aes 256
 hash sha
 authentication pre-share
 group 14
 lifetime 86400
crypto isakmp key VPN-PME2026 address 203.0.113.1
crypto ipsec transform-set TS-GE-LS esp-aes 256 esp-sha-hmac
 mode tunnel
ip access-list extended VPN-TRAFIC
 permit ip 10.2.0.0 0.0.255.255 10.1.0.0 0.0.255.255
crypto map CMAP-VPN 10 ipsec-isakmp
 set peer 203.0.113.1
 set transform-set TS-GE-LS
 match address VPN-TRAFIC
interface Gi1/0
 crypto map CMAP-VPN
```

![image](https://hackmd.io/_uploads/BJWvPj12Gl.png)


> Le message `ISAKMP is ON` confirme l'activation du VPN. Les `$` affichés en début de ligne indiquent seulement que la commande était trop longue pour la console.

---

## 8. Tests et résultats

### 8.1 Routage inter-VLAN (local)

| Test | Résultat attendu | Résultat |
|---|---|---|
| PC-EMP-GE → SVR-FICHIERS (VLAN 20 → 99) | Réussi | ✅ |
| PC-GENEVE → PC-EMP-GE (VLAN 10 → 20) | Réussi | ✅ |
| PC-LAUSANNE → PC-EMP-LSN (VLAN 10 → 20) | Réussi | ✅ |

![image](https://hackmd.io/_uploads/ryNOwskhze.png)


![image](https://hackmd.io/_uploads/HJEFDjkhGe.png)


![image](https://hackmd.io/_uploads/HJQcvjy2zg.png)


> **ttl=63** : le paquet est parti avec un TTL de 64 et n'a traversé qu'un seul routeur (le routeur local qui fait le routage inter-VLAN).

### 8.2 Avant le VPN

| Test | Résultat attendu | Résultat |
|---|---|---|
| PC-LAUSANNE → SVR-FICHIERS | Échec | ❌ timeout |

![image](https://hackmd.io/_uploads/BkFiwo1nMx.png)


> R-LAUSANNE envoie le paquet vers R-ISP grâce à la route par défaut, mais R-ISP ne connaît pas le réseau 10.1.99.0 et le jette. Les adresses privées ne sont pas routées sur Internet.

### 8.3 Après le VPN

| Test | Résultat attendu | Résultat |
|---|---|---|
| PC-LAUSANNE → SVR-FICHIERS | Réussi | ✅ |
| `show crypto isakmp sa` | QM_IDLE / ACTIVE | ✅ |
| `show crypto ipsec sa` | encaps / decaps > 0, 0 erreur | ✅ 9 / 9 |
| R-ISP → 10.1.10.11 | Échec | ❌ 0 % (voulu) |

![image](https://hackmd.io/_uploads/HyF2DoJnMl.png)


> - Les **3 premiers timeouts** correspondent à la négociation du tunnel (phase 1 puis phase 2). Au 2e ping : 100 %.
> - **ttl=62** : seulement 2 routeurs traversés (R-LAUSANNE et R-GENEVE). R-ISP ne voit qu'un paquet chiffré (ESP) entre les IP publiques et ne touche pas au paquet d'origine. C'est la preuve que le trafic passe dans le tunnel.

![image](https://hackmd.io/_uploads/Syc6wik3fx.png)


> État **QM_IDLE / ACTIVE** : la phase 1 est établie. **src = 198.51.100.1**, c'est Lausanne qui a déclenché le tunnel.

![image](https://hackmd.io/_uploads/SyVydok2Ml.png)


> - **local ident 10.1.0.0/16 ↔ remote ident 10.2.0.0/16** : correspond exactement à l'ACL VPN-TRAFIC.
> - **9 paquets encapsulés / 9 décapsulés**, aucune erreur : le chiffrement et le déchiffrement fonctionnent dans les deux sens.

![image](https://hackmd.io/_uploads/Hkdg_jJhfg.png)


> R-ISP (« Internet ») ne peut pas joindre les réseaux internes : ils ne sont accessibles **que par le tunnel VPN**.

---

## 9. Problème rencontré

| Symptôme | Cause | Solution |
|---|---|---|
| `ping 198.51.100.1` depuis R-GENEVE : 0 % | La route par défaut n'avait pas été saisie sur R-GENEVE | Ajout de `ip route 0.0.0.0 0.0.0.0 203.0.113.2`, puis ping à 100 % |

> **À retenir** : sans route, un routeur ne connaît que ses réseaux directement connectés.

---

## 10. Synthèse des choix techniques

| Choix | Justification |
|---|---|
| Adressage 10.site.VLAN.0/24 | Lisible, évolutif, une seule ligne d'ACL par site pour le VPN |
| VLAN ADMIN / EMPLOYES / SERVEURS | Séparation des flux, sécurité, moins de broadcasts |
| Router-on-a-stick | Économique : pas besoin de switch L3 pour une PME |
| VPN IPsec site-à-site | Sécurisé (AES-256) et bon marché par rapport à une ligne dédiée |
| Groupe DH 14 | Plus sûr que les groupes 1, 2 ou 5, devenus trop faibles |
| ACL VPN limitée à 10.1.0.0/16 ↔ 10.2.0.0/16 | Seul le trafic inter-sites est chiffré, le reste peut aller sur Internet |

---

## 11. Améliorations possibles

- **NAT** pour l'accès Internet des postes, en excluant le trafic VPN du NAT.
- **ACL inter-VLAN**, par exemple pour limiter l'accès des employés au VLAN SERVEURS.
- **DHCP** sur les routeurs au lieu des IP fixes sur les postes.
- **Authentification par certificats** à la place de la clé pré-partagée.
- **Redondance** : deuxième accès Internet ou ligne de secours.

---

## 12. Conclusion

L'architecture conçue répond aux besoins de la PME :
- chaque site est segmenté en VLAN avec un routage inter-VLAN fonctionnel ;
- le plan d'adressage est cohérent et prêt pour un 3e site ;
- les deux sites communiquent de façon **chiffrée** grâce au VPN IPsec site-à-site ;
- les réseaux internes restent invisibles depuis Internet.

Tous les tests ont été validés sur la maquette GNS3.

*Negreche Sofia E1A