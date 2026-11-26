# Projet 3 — Mise en place d'un routeur/pare-feu pour une petite entreprise

**Cours :** Networking Essentials — Infrastructure informatique et réseau d'une petite entreprise
**Classe :** E1A — **Niveau :** Intermédiaire — **Durée :** 1 semaine (PPE)
**Outil :** GNS3 (projet `NE-P3-Pare-feu`)
**Auteur :** Negreche Sofia

[TOC]

---

## 1. Contexte et objectifs

Une petite entreprise souhaite connecter son réseau local à Internet de façon sécurisée, avec un pare-feu basique.

Objectifs :
- configurer un routeur pour la connexion Internet (interfaces WAN/LAN, NAT) ;
- mettre en place des règles de pare-feu simples (blocage d'un port/service) ;
- tester l'accès Internet et le blocage du trafic non autorisé.

---

## 2. Topologie

![image](https://hackmd.io/_uploads/B1ricTnoGg.png)


```
   PC1 (192.168.10.11)      PC2 (192.168.10.12)
        e0 |                     | e0
        e0 |                     | e1
           [       Switch1       ]
                     | e2
              Gi0/0  | 192.168.10.1     (LAN - nat inside)
              [     R1 c7200     ]      ACL PAREFEU-LAN (in sur Gi0/0)
              Gi1/0  | 192.168.208.141  (WAN - DHCP - nat outside)
                     | nat0
                 [  NAT1  ] ── Internet
```

| Équipement | Type GNS3 | Rôle |
|---|---|---|
| R1 | Routeur Cisco c7200 (slot 0 : C7200-IO-GE-E, slot 1 : PA-GE) | Routeur + NAT + pare-feu (ACL) |
| Switch1 | Ethernet switch GNS3 | Commutateur du LAN |
| PC1, PC2 | VPCS | Postes de l'entreprise |
| NAT1 | Nœud NAT GNS3 | Accès Internet (simule le FAI) |

| Lien | Ports |
|---|---|
| PC1 ↔ Switch1 | e0 ↔ e0 |
| PC2 ↔ Switch1 | e0 ↔ e1 |
| Switch1 ↔ R1 | e2 ↔ Gi0/0 |
| R1 ↔ NAT1 | Gi1/0 ↔ nat0 |

---

## 3. Plan d'adressage

| Équipement | Interface | Adresse IP | Masque | Passerelle | DNS |
|---|---|---|---|---|---|
| R1 | Gi0/0 (LAN) | 192.168.10.1 | 255.255.255.0 | — | — |
| R1 | Gi1/0 (WAN) | 192.168.208.141 (DHCP) | 255.255.255.0 | 192.168.208.2 | — |
| PC1 | e0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 | 8.8.8.8 |
| PC2 | e0 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 | 8.8.8.8 |

---

## 4. Configuration de R1

### 4.1 Interface LAN (Gi0/0)

```
enable
configure terminal
interface GigabitEthernet0/0
 description LAN - Reseau entreprise
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
 no shutdown
 exit
```

- `description` : indique le rôle de l'interface. Ce texte apparaît dans `show interfaces` et facilite le dépannage.
- `ip address` : adresse du routeur côté LAN. C'est la passerelle par défaut des PC.
- `ip nat inside` : déclare l'interface comme côté **interne** du NAT, c'est-à-dire les adresses privées à traduire.
- `no shutdown` : active l'interface, qui est désactivée par défaut sur un routeur Cisco.

### 4.2 Interface WAN (Gi1/0)

```
interface GigabitEthernet1/0
 description WAN - Internet
 ip address dhcp
 ip nat outside
 no shutdown
 exit
end
```

- `ip address dhcp` : le routeur reçoit son adresse côté Internet par DHCP, depuis le nœud NAT de GNS3 qui joue le rôle du FAI. Adresse obtenue : **192.168.208.141/24**.
- `ip nat outside` : déclare l'interface comme côté **externe** du NAT, celui vers lequel les adresses sont traduites.

![02-config-interfaces-R1](https://hackmd.io/_uploads/ByzT5phizl.png)


Vérification avec `show ip interface brief` : Gi0/0 (192.168.10.1, manual) et Gi1/0 (192.168.208.141, DHCP) sont **up/up**.

![03-ip-wan-dhcp-show-ip-int-brief](https://hackmd.io/_uploads/r1SRc6hjMg.png)


### 4.3 Route par défaut

Aucune route statique n'a été ajoutée. La route par défaut a été **installée automatiquement par le DHCP** :

```
S*    0.0.0.0/0 [254/0] via 192.168.208.2
```

- `S*` : route statique, candidate par défaut.
- `[254/0]` : distance administrative de 254. C'est la valeur utilisée par IOS pour une route par défaut apprise par DHCP.
- `via 192.168.208.2` : la passerelle du FAI.

Test depuis R1 : `ping 8.8.8.8` donne **5/5 (100 %)**. Le routeur a bien accès à Internet.

![04-route-par-defaut-ping-R1](https://hackmd.io/_uploads/rktysahsMg.png)


### 4.4 NAT (PAT / overload)

```
configure terminal
access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface GigabitEthernet1/0 overload
end
write memory
```

- `access-list 1` : ACL standard qui désigne **quelles adresses ont le droit d'être traduites**, ici tout le réseau 192.168.10.0/24. `0.0.0.255` est le masque générique (wildcard).
- `ip nat inside source list 1 interface Gi1/0` : les adresses de la liste 1 sont traduites par l'adresse de l'interface WAN (192.168.208.141).
- `overload` : active le **PAT**. Tous les PC partagent la même adresse publique, et le routeur les distingue par le numéro de port.
- `write memory` : sauvegarde la configuration dans la NVRAM. L'avertissement « different version of the system image » est sans conséquence.

![05-config-nat](https://hackmd.io/_uploads/rylZjphsze.png)


### 4.5 Pare-feu : ACL étendue PAREFEU-LAN

```
configure terminal
ip access-list extended PAREFEU-LAN
 deny tcp any any eq 80
 deny tcp any any eq 23
 permit ip any any
 exit
interface GigabitEthernet0/0
 ip access-group PAREFEU-LAN in
 exit
end
write memory
```

- `ip access-list extended PAREFEU-LAN` : crée une ACL **étendue nommée**. Contrairement à une ACL standard, elle peut filtrer selon le protocole et le port de destination.
- `deny tcp any any eq 80` : bloque le **HTTP** (web non sécurisé). Affiché `eq www` par IOS.
- `deny tcp any any eq 23` : bloque le **Telnet** (accès à distance non chiffré). Affiché `eq telnet` par IOS.
- `permit ip any any` : autorise tout le reste. Cette ligne est indispensable, car une ACL se termine par un **deny implicite** qui bloquerait tout le trafic.
- `ip access-group PAREFEU-LAN in` : applique l'ACL **en entrée sur Gi0/0**. Le trafic des PC est filtré dès son arrivée sur le routeur, avant d'être routé vers Internet.

> Les règles sont lues **de haut en bas** et la première qui correspond s'applique. C'est pour cela que les `deny` sont placés avant le `permit`.

![12-config-acl-parefeu-lan](https://hackmd.io/_uploads/rJIGiT2iGe.png)


---

## 5. Configuration des PC (VPCS)

**PC1 :**
```
ip 192.168.10.11 255.255.255.0 192.168.10.1
ip dns 8.8.8.8
save
show ip
```

![06-config-ip-PC1](https://hackmd.io/_uploads/rJgEi6nsfe.png)


**PC2 :**
```
ip 192.168.10.12 255.255.255.0 192.168.10.1
ip dns 8.8.8.8
save
show ip
```

![07-config-ip-PC2](https://hackmd.io/_uploads/BJUBjTnifx.png)


- `ip <adresse> <masque> <passerelle>` : configure l'adresse IP, le masque et la passerelle (R1).
- `ip dns 8.8.8.8` : serveur DNS de Google, qui permet de résoudre les noms de domaine.
- `save` : enregistre la configuration dans `startup.vpc`. Elle est conservée au redémarrage du PC.

---

## 6. Tests

> Commande de test de port avec VPCS : `ping <destination> -P 6 -p <port>`
> `-P 6` (majuscule) envoie des paquets **TCP** (protocole n° 6) au lieu d'ICMP, et `-p` (minuscule) choisit le port de destination.

### 6.1 Avant le pare-feu (connexion Internet et NAT)

| N° | Test | Poste | Commande | Résultat attendu | Résultat obtenu | Capture |
|---|---|---|---|---|---|---|
| 1 | Ping Internet | PC1 | `ping 8.8.8.8` | Réponses | 5/5, TTL 127 | 08 |
| 2 | Ping de la passerelle | PC1 | `ping 192.168.10.1` | Réponses | 5/5 | 09 |
| 3 | Résolution DNS | PC1 | `ping google.com` | Nom résolu + réponses | résolu en 172.217.208.101 | 09 |
| 4 | Port 443 (HTTPS) | PC1 | `ping google.com -P 6 -p 443` | Connexion TCP établie | Connect / SendData / Close | 11 |
| 5 | Port 80 (HTTP) | PC1 | `ping google.com -P 6 -p 80` | Connexion TCP établie | Connect / SendData / Close | 11 |

![08-PC1-ping-internet](https://hackmd.io/_uploads/HJbPo6noGl.png)

![09-PC1-ping-passerelle-dns](https://hackmd.io/_uploads/Sk4_iphoGx.png)

![11-ports-443-80-ouverts-avant-acl](https://hackmd.io/_uploads/ByUtsTnoMg.png)


> Le TTL de 127 (au lieu de 128) montre que le paquet a traversé un routeur. Quelques `timeout` isolés sont normaux : ce sont des pertes ponctuelles à travers le NAT de VMware.

**Vérification du NAT** sur R1, pendant un ping continu (`ping 8.8.8.8 -t`) depuis PC1 :

```
show ip nat translations
```

| Inside local | Inside global | Outside global |
|---|---|---|
| 192.168.10.11:59197 | 192.168.208.141:1024 | 8.8.8.8:1024 |
| 192.168.10.11:59453 | 192.168.208.141:1025 | 8.8.8.8:1025 |
| … | … | … |

L'adresse privée de PC1 (192.168.10.11) est traduite en adresse WAN de R1 (192.168.208.141). Chaque échange reçoit un **port différent** (1024, 1025, 1026…) : c'est le principe du **PAT**.

![10-table-nat-show-ip-nat-translations](https://hackmd.io/_uploads/ByujiTnofl.png)


### 6.2 Après le pare-feu (blocage)

| N° | Test | Poste | Commande | Résultat attendu | Résultat obtenu | Capture |
|---|---|---|---|---|---|---|
| 6 | HTTP (port 80) | PC2 | `ping google.com -P 6 -p 80` | bloqué | bloqué : *ICMP type 3, code 13 — Communication administratively prohibited* | 13 |
| 7 | HTTPS (port 443) | PC1 | `ping google.com -P 6 -p 443` | autorisé | Connect / SendData / Close | 14 |
| 8 | Telnet (port 23) | PC1 | `ping 8.8.8.8 -P 6 -p 23` | bloqué | bloqué : *ICMP type 3, code 13 — Communication administratively prohibited* | 15 |
| 9 | Ping Internet | PC1 | `ping 8.8.8.8` | autorisé | réponses | 17 |

![13-PC2-port80-bloque](https://hackmd.io/_uploads/BJf6sp2jze.png)

![14-PC1-port443-autorise-apres-acl](https://hackmd.io/_uploads/HyjRs63szl.png)

![15-PC1-port23-bloque](https://hackmd.io/_uploads/SJp1n63jfe.png)

![image](https://hackmd.io/_uploads/rkxmnT2ozg.png)


> Le message **ICMP type 3, code 13 « Communication administratively prohibited »** est envoyé par **R1 lui-même** (192.168.10.1). Le routeur indique au PC que le trafic a été refusé par une règle de filtrage. Seules les réponses 1, 3 et 5 s'affichent, car IOS limite le nombre de messages ICMP d'erreur qu'il envoie (*rate-limiting*). C'est un comportement normal.

**Vérification des compteurs de l'ACL** sur R1 :

```
show access-lists
```
```
Standard IP access list 1
    10 permit 192.168.10.0, wildcard bits 0.0.0.255 (90 matches)
Extended IP access list PAREFEU-LAN
    10 deny tcp any any eq www (3 matches)
    20 deny tcp any any eq telnet (3 matches)
    30 permit ip any any (32 matches)
```

- **3 matches sur `www`** : les 3 tentatives HTTP de PC2 ont été interceptées et bloquées.
- **3 matches sur `telnet`** : les 3 tentatives Telnet de PC1 ont été bloquées.
- **32 matches sur `permit`** : le reste du trafic (HTTPS, DNS, ICMP) passe normalement.
- **90 matches sur l'access-list 1** : nombre de paquets du LAN pris en charge par le NAT.

![16-compteurs-show-access-lists](https://hackmd.io/_uploads/H1yBn62sGx.png)


---

## 7. Conclusion

Le routeur R1 relie le LAN de l'entreprise (192.168.10.0/24) à Internet. Il reçoit son adresse WAN (192.168.208.141) et sa route par défaut par DHCP. Le NAT/PAT permet à tous les postes de partager cette seule adresse. L'ACL étendue PAREFEU-LAN, appliquée en entrée sur l'interface LAN, joue le rôle de pare-feu basique : elle bloque HTTP (port 80) et Telnet (port 23), deux protocoles non chiffrés, et laisse passer le reste du trafic, notamment HTTPS.

Les tests le confirment : le routeur répond « administratively prohibited » sur les ports interdits, et les compteurs `show access-lists` montrent les paquets bloqués et autorisés.

Amélioration possible : passer à une logique « tout bloquer sauf l'autorisé », c'est-à-dire autoriser seulement DNS (53), HTTPS (443) et ICMP, puis tout refuser. C'est plus sûr qu'une liste de ports interdits.

---

## 8. Liste des captures

| N° | Contenu |
|---|---|
| 01 | Topologie GNS3 |
| 02 | Configuration des interfaces Gi0/0 et Gi1/0 de R1 |
| 03 | Adresse WAN obtenue en DHCP + `show ip interface brief` |
| 04 | Table de routage (route par défaut DHCP) + ping 8.8.8.8 depuis R1 |
| 05 | Configuration du NAT |
| 06 | Configuration IP de PC1 |
| 07 | Configuration IP de PC2 |
| 08 | Ping Internet depuis PC1 |
| 09 | Ping de la passerelle et résolution DNS depuis PC1 |
| 10 | Table NAT (`show ip nat translations`) |
| 11 | Ports 443 et 80 ouverts avant le pare-feu |
| 12 | Configuration de l'ACL PAREFEU-LAN |
| 13 | Port 80 bloqué depuis PC2 |
| 14 | Port 443 autorisé depuis PC1 après l'ACL |
| 15 | Port 23 bloqué depuis PC1 |
| 16 | Compteurs `show access-lists` |
| 17 | Ping Internet depuis PC1 après l'ACL |
