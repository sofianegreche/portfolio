# TP ICND1 – Interconnexion des sites et sécurisation des accès

**Étudiante :** Sofia Negreche
**Formation :** Bachelor IT – Geneva Institute of Technology
**Client :** Léman Logistique SA – Ticket SAT-2026-0412
**Prestataire :** Satom IT & Learning Solutions
**Date d'intervention :** 03.10.2026

---

## Sommaire

- Contexte
- Maquette
- Plan d'adressage
- Interfaces des routeurs
- Partie 1 – Interconnexion des sites et intégration VMware
- Partie 2 – Sécurisation des accès (Telnet / SSH)
- Démonstration Wireshark Telnet / SSH
- Comparatif Telnet / SSH
- Récapitulatif des incidents
- Bilan général
- Fichiers joints

---

## Contexte

Léman Logistique SA souhaite relier deux sites (LAN-1 et LAN-2) par l'intermédiaire d'un routeur central qui assure aussi l'accès à Internet. Ce document regroupe :

- **Partie 1** : raccordement de deux postes clients (VM VMware) à la maquette GNS3, routage statique entre les sites et accès Internet par NAT.
- **Partie 2** : sécurisation des routeurs (mots de passe, bannière, SSH, Telnet), puis comparaison de Telnet et SSH sur une capture Wireshark.

---

## Maquette

La maquette tourne dans GNS3 avec trois routeurs Cisco c7200. Les deux postes clients sont des VM VMware Workstation, reliées à GNS3 par des nœuds Cloud :

- **PC1** : Windows 11, sur VMnet1, derrière R1
- **PC2** : Ubuntu, sur VMnet2, derrière R3
- **Internet** : réseau NAT de VMware (VMnet8), branché sur R2

![image](https://hackmd.io/_uploads/SyZyq5Ccze.png)

---

## Plan d'adressage

| Segment | Réseau | Masque | Équipements |
|---|---|---|---|
| LAN-1 (PC1) | 192.168.20.0 /26 | 255.255.255.192 | R1 .1, PC1 .10, carte VMnet1 de l'hôte .61 |
| Liaison R1–R2 | 192.168.21.0 /30 | 255.255.255.252 | R1 .1, R2 .2 |
| Liaison R2–R3 | 192.168.22.0 /30 | 255.255.255.252 | R3 .1, R2 .2 |
| LAN-2 (PC2) | 192.168.23.0 /26 | 255.255.255.192 | R3 .1, PC2 .10, carte VMnet2 de l'hôte .61 |
| Internet | 192.168.208.0 /24 | 255.255.255.0 | R2 en DHCP (réseau NAT VMware, VMnet8) |

Les LAN sont en /26 (62 hôtes possibles). Les liaisons entre routeurs sont en /30, car elles n'ont besoin que de deux adresses.

## Interfaces des routeurs

| Routeur | Interface | Rôle | Adresse |
|---|---|---|---|
| R1 | Gi0/0 | Passerelle LAN-1 | 192.168.20.1 /26 |
| R1 | Gi1/0 | Liaison vers R2 | 192.168.21.1 /30 |
| R2 | Gi0/0 | Liaison vers R1 (NAT inside) | 192.168.21.2 /30 |
| R2 | Gi1/0 | Liaison vers R3 (NAT inside) | 192.168.22.2 /30 |
| R2 | Gi2/0 | Accès Internet (NAT outside) | DHCP → 192.168.208.138 |
| R3 | Gi1/0 | Liaison vers R2 | 192.168.22.1 /30 |
| R3 | Gi0/0 | Passerelle LAN-2 | 192.168.23.1 /26 |

---

## Partie 1 – Interconnexion des sites et intégration VMware

### Préparation de VMware Workstation

VMnet1 et VMnet2 sont des commutateurs virtuels qui relient chaque VM à l'interface LAN d'un routeur GNS3. Le DHCP de VMware est désactivé pour que les VM ne reçoivent pas une adresse hors du plan.

| | VMnet1 (LAN-1) | VMnet2 (LAN-2) |
|---|---|---|
| Type | Host-only | Host-only |
| DHCP VMware | désactivé | désactivé |
| Réseau | 192.168.20.0 /26 | 192.168.23.0 /26 |

![Capture d'écran 2026-10-03 171300](https://hackmd.io/_uploads/S1lijqR9fg.png)
![Capture d'écran 2026-10-03 171252](https://hackmd.io/_uploads/rk-is5CcMx.png)

### Cartes VMnet de l'hôte Windows

Par défaut, VMware donne à la carte de l'hôte la première adresse du réseau (.1), déjà utilisée par la passerelle. Pour éviter le conflit, chaque carte reçoit une adresse fixe en .61.

| Carte de l'hôte | Adresse | Masque |
|---|---|---|
| VMware Network Adapter VMnet1 | 192.168.20.61 | 255.255.255.192 |
| VMware Network Adapter VMnet2 | 192.168.23.61 | 255.255.255.192 |


![image](https://hackmd.io/_uploads/BySh250qMx.png)
![image](https://hackmd.io/_uploads/SJqbacCcfx.png)


### Nœuds Cloud dans GNS3

| Nœud Cloud | Carte liée | Branché sur |
|---|---|---|
| Cloud1 | VMware Network Adapter VMnet1 | R1 Gi0/0 |
| Cloud2 | VMware Network Adapter VMnet2 | R3 Gi0/0 |
| Cloud3 | VMware Network Adapter VMnet8 (NAT) | R2 Gi2/0 |

![image](https://hackmd.io/_uploads/SyZyq5Ccze.png)

### État de départ des routeurs

Au démarrage, les trois routeurs n'avaient aucune configuration : toutes les interfaces étaient « unassigned / administratively down » (voir incident 1).

📸 **09 – R1, R2, R3 : show ip interface brief avant configuration**
`09_routeurs-vierges`![Capture d'écran 2026-10-03 150536](https://hackmd.io/_uploads/rkiw6cA9Me.png)![Capture d'écran 2026-10-03 151010](https://hackmd.io/_uploads/Hk1Fa509zl.png)
![Capture d'écran 2026-10-03 151017](https://hackmd.io/_uploads/Sykta909Gl.png)



### Configuration des interfaces

**R1**
```
interface GigabitEthernet0/0
 ip address 192.168.20.1 255.255.255.192
 no shutdown
interface GigabitEthernet1/0
 ip address 192.168.21.1 255.255.255.252
 no shutdown
```

**R2**
```
interface GigabitEthernet0/0
 ip address 192.168.21.2 255.255.255.252
 ip nat inside
 no shutdown
interface GigabitEthernet1/0
 ip address 192.168.22.2 255.255.255.252
 ip nat inside
 no shutdown
interface GigabitEthernet2/0
 ip address dhcp
 ip nat outside
 no shutdown
```

**R3**
```
interface GigabitEthernet0/0
 ip address 192.168.23.1 255.255.255.192
 no shutdown
interface GigabitEthernet1/0
 ip address 192.168.22.1 255.255.255.252
 no shutdown
```

![Capture d'écran 2026-10-03 172338](https://hackmd.io/_uploads/HJqf0qRqMe.png)
![Capture d'écran 2026-10-03 172310](https://hackmd.io/_uploads/rkqGAqA5ze.png)
![Capture d'écran 2026-10-03 172252](https://hackmd.io/_uploads/rkczR50qfl.png)

### Routage statique

Un routeur ne connaît que les réseaux branchés sur lui. Les routes statiques lui indiquent par quel voisin passer pour atteindre les autres.

**R1** : une seule sortie, vers R2, donc une route par défaut suffit.
```
ip route 0.0.0.0 0.0.0.0 192.168.21.2
```

**R3** : même principe.
```
ip route 0.0.0.0 0.0.0.0 192.168.22.2
```

**R2** : il doit connaître les deux LAN. Sa route par défaut vers Internet est apprise automatiquement par DHCP.
```
ip route 192.168.20.0 255.255.255.192 192.168.21.1
ip route 192.168.23.0 255.255.255.192 192.168.22.1
```

### NAT (accès Internet)

Les adresses des LAN sont privées et inconnues du réseau VMware. R2 les traduit en sa propre adresse Internet (NAT overload, ou PAT).

```
access-list 1 permit 192.168.20.0 0.0.0.63
access-list 1 permit 192.168.23.0 0.0.0.63
ip nat inside source list 1 interface GigabitEthernet2/0 overload
```

### Tables de routage

![image](https://hackmd.io/_uploads/H1AwR9RcGg.png)

| Ligne | Signification |
|---|---|
| S* 0.0.0.0/0 [254/0] via 192.168.208.2 | Route par défaut vers Internet, apprise par DHCP (distance 254) |
| S 192.168.20.0/26 [1/0] via 192.168.21.1 | LAN-1, en passant par R1 |
| S 192.168.23.0/26 [1/0] via 192.168.22.1 | LAN-2, en passant par R3 |
| C 192.168.21.0/30 / C 192.168.22.0/30 | Liaisons vers R1 et R3, branchées directement |

![Capture d'écran 2026-10-03 172622](https://hackmd.io/_uploads/HyA3RcRqGe.png)
![Capture d'écran 2026-10-03 172600](https://hackmd.io/_uploads/HyAhCcR5zx.png)


Codes : C = réseau directement connecté, L = adresse du routeur lui-même, S = route statique, S* = route par défaut. Le premier chiffre entre crochets est la distance administrative.

### Tests depuis les routeurs

![image](https://hackmd.io/_uploads/rJ8l1sRqfx.png)


| Depuis | Vers | Résultat |
|---|---|---|
| R1 | R3, côté LAN-2 (192.168.23.1) | ✅ |
| R1 (source LAN-1) | Internet (8.8.8.8) | ✅ |

Le ping vers 8.8.8.8 est lancé avec `source g0/0` pour partir de l'adresse du LAN : il prouve que le routage et le NAT de R2 fonctionnent.

### Postes clients

| Paramètre | PC1 (Windows 11) | PC2 (Ubuntu) |
|---|---|---|
| Carte VMware | Custom : VMnet1 | Custom : VMnet2 |
| Adresse IP | 192.168.20.10 | 192.168.23.10 |
| Masque | 255.255.255.192 | 255.255.255.192 (/26) |
| Passerelle | 192.168.20.1 (R1) | 192.168.23.1 (R3) |
| DNS | 8.8.8.8 | 8.8.8.8 |

![image](https://hackmd.io/_uploads/Ske81oAcGl.png)


![image](https://hackmd.io/_uploads/HyI7eiR9Gl.png)

![image](https://hackmd.io/_uploads/H1d21oRqMl.png)


Sur Ubuntu, aucune connexion n'était active sur la carte ens33 (incident 5). La configuration a été créée en ligne de commande :
```
sudo nmcli con add type ethernet ifname ens33 con-name TP ipv4.method manual ipv4.addresses 192.168.23.10/26 ipv4.gateway 192.168.23.1 ipv4.dns 8.8.8.8
sudo nmcli con up TP
```

![image](https://hackmd.io/_uploads/HJYsxiC9zx.png)

Pour que PC1 réponde au ping, le pare-feu Windows a été ouvert à l'ICMP (incident 6) :
```
netsh advfirewall firewall add rule name="ICMP" protocol=icmpv4:8,any dir=in action=allow
```

📸 **21 – PC1 : règle pare-feu ICMP ajoutée (Ok.)**
`21_PC1_netsh-ICMP`

### Tests depuis les postes

![image](https://hackmd.io/_uploads/rJOzZjAcMl.png)


![image](https://hackmd.io/_uploads/Byyybi05zl.png)

![image](https://hackmd.io/_uploads/B1kVboC5fe.png)

![image](https://hackmd.io/_uploads/B1Wrbi0qGl.png)
![image](https://hackmd.io/_uploads/r1F8bj05fe.png)

| Depuis | Vers | Résultat |
|---|---|---|
| PC2 | Passerelle R3 (192.168.23.1) | ✅ |
| PC2 | PC1 (192.168.20.10) | ✅ |
| PC1 | PC2 (192.168.23.10) | ✅ |
| PC1 / PC2 | Internet (8.8.8.8) | ✅ |
| PC1 / PC2 | google.com | ❌ résolution DNS impossible (incident 4) |

### Bilan de la partie 1

| Élément | Résultat |
|---|---|
| VMnet1 et VMnet2 en Host-only, DHCP désactivé | ✅ |
| Trois nœuds Cloud : VMnet1, VMnet2, VMnet8 | ✅ |
| Interfaces des 3 routeurs up/up | ✅ |
| Routage statique | ✅ |
| NAT sur R2, accès Internet par IP | ✅ |
| Communication PC1 ↔ PC2 | ✅ |
| Résolution DNS depuis les VM | ❌ (limite VMware, voir incident 4) |

---

## Partie 2 – Sécurisation des accès

### Configuration de sécurité de base (sur R1, R2 et R3)

```
service password-encryption
enable secret cisco
banner motd #Acces reserve - Leman Logistique#
line con 0
 password cisco
 login
```


### Configuration SSH et Telnet (sur R1, R2 et R3)

```
ip domain-name leman.local
username admin privilege 15 secret cisco
crypto key generate rsa modulus 1024
ip ssh version 2
line vty 0 4
 login local
 transport input ssh telnet
```

Les lignes VTY acceptent Telnet et SSH pour pouvoir comparer les deux protocoles. L'authentification se fait avec le compte local `admin`.

![image](https://hackmd.io/_uploads/Hk4lXiR9zg.png)
![image](https://hackmd.io/_uploads/By-QXjC9fl.png)
![image](https://hackmd.io/_uploads/ry3rQjR9Mx.png)

### Connexions SSH depuis les postes

Le client SSH de Windows 11 et d'Ubuntu refuse par défaut les anciens algorithmes proposés par le routeur (incident 7). Il faut les autoriser dans la commande :

```
ssh -oKexAlgorithms=+diffie-hellman-group14-sha1 -oHostKeyAlgorithms=+ssh-rsa -oPubkeyAcceptedAlgorithms=+ssh-rsa -c aes128-cbc -m hmac-sha1 admin@192.168.20.1
```
![image](https://hackmd.io/_uploads/Bkwd4oA9fg.png)

📸 **32![image](https://hackmd.io/_uploads/H155NsA5Me.png)


### Connexions Telnet depuis les postes

Sous Windows, le client Telnet a été activé au préalable :
```
dism /online /Enable-Feature /FeatureName:TelnetClient
```

![image](https://hackmd.io/_uploads/ByegHjRcfl.png)

![image](https://hackmd.io/_uploads/SJ1friR5fg.png)
![image](https://hackmd.io/_uploads/B1yoSo09fl.png)

![image](https://hackmd.io/_uploads/SJgRHiRqfe.png)

| Test | Résultat |
|---|---|
| SSH PC1 → R1 | ✅ |
| SSH PC2 → R3 | ✅ |
| Telnet PC1 → R3 | ✅ |
| Telnet PC2 → R1 | ✅ |

---

## Démonstration Wireshark Telnet / SSH

La capture est faite dans GNS3 sur le lien **Cloud1 ↔ R1 Gi0/0** (clic droit sur le lien, puis Start capture). Les deux connexions partent de PC1 (192.168.20.10) vers R1 (192.168.20.1).

![image](https://hackmd.io/_uploads/HkNB8jAqMg.png)

### Telnet

Filtre `telnet`, connexion `telnet 192.168.20.1`, puis clic droit sur un paquet → Suivre → Flux TCP.

![image](https://hackmd.io/_uploads/S1Fn8iRqMg.png)


Toute la session est en clair : la bannière, l'identifiant `admin` (les lettres apparaissent en double parce que le routeur renvoie chaque caractère en écho), le mot de passe `cisco` et les commandes tapées. Le mot de passe ne s'affiche pas à l'écran de l'utilisateur, mais il passe en clair sur le réseau.

![image](https://hackmd.io/_uploads/Sk85vsAqfe.png)
![image](https://hackmd.io/_uploads/SJ6cPiC5fx.png)
![image](https://hackmd.io/_uploads/SknoPsA5fl.png)

### SSH

Filtre `ssh`, connexion SSH vers 192.168.20.1, puis Suivre → Flux TCP.

![image](https://hackmd.io/_uploads/Bk0WdiAqfg.png)

![image](https://hackmd.io/_uploads/HkfQOiRqMx.png)

Seul le début de l'échange est lisible (version SSH et échange de clés). Ensuite, ni l'identifiant, ni le mot de passe, ni les commandes ne sont visibles.


---

## Comparatif Telnet / SSH

| Critère | Telnet | SSH v2 |
|---|---|---|
| Port | TCP 23 | TCP 22 |
| Chiffrement | Aucun, tout passe en clair | AES-128-CBC, intégrité HMAC-SHA1, échange de clés Diffie-Hellman group14 |
| Authentification | Compte local, mot de passe envoyé en clair | Compte local, mot de passe envoyé chiffré |
| Identité du routeur | Pas vérifiée | Clé RSA présentée par le routeur |
| Visible dans Wireshark | Bannière, identifiant, mot de passe, commandes | Version SSH et échange de clés uniquement |

### Recommandation

Telnet ne doit pas être utilisé pour administrer les routeurs : n'importe qui écoutant le réseau récupère l'identifiant et le mot de passe. Il est recommandé de :

- n'autoriser que SSH sur les lignes VTY (`transport input ssh`) ;
- passer la clé RSA à 2048 bits (`crypto key generate rsa modulus 2048`) ;
- utiliser des mots de passe forts à la place de `cisco` ;
- limiter les adresses autorisées en SSH avec une ACL (`access-class`) ;
- ajouter une déconnexion automatique (`exec-timeout`) sur la console et les VTY.

---

## Récapitulatif des incidents

| N° | Équipement | Symptôme | Cause | Résolution |
|---|---|---|---|---|
| 1 | R1, R2, R3 | Toutes les interfaces « unassigned / administratively down » | Configuration jamais appliquée ou pas sauvegardée. De plus, le schéma de départ contenait des erreurs : même IP sur les deux LAN, masque 255.255.255.254 sur un LAN, masques différents des deux côtés de la liaison R1–R2 | Nouveau plan d'adressage cohérent, configuration complète puis `write` |
| 2 | PC2 (Windows 11) | Ping de la passerelle : « Impossible de joindre l'hôte », réponse de 192.168.13.10 | Ancienne adresse IP restée sur la VM | IP corrigée en 192.168.23.10 |
| 3 | PC2 (Windows 11) | VM non fonctionnelle | — | Remplacée par une VM Ubuntu |
| 4 | PC1 / PC2 | `ping 8.8.8.8` OK mais `ping google.com` échoue ; `nslookup google.com 8.8.8.8` en délai dépassé depuis les VM, mais OK depuis l'hôte | Requêtes DNS bloquées entre les VM et l'hôte, côté NAT VMware | Non résolu. Non bloquant : l'accès Internet par IP prouve que le NAT fonctionne |
| 5 | PC2 (Ubuntu) | `ping` : « Le réseau n'est pas accessible » | Aucune connexion active sur ens33, donc pas d'adresse IPv4 | Connexion « TP » créée avec `nmcli` |
| 6 | PC1 | Ping PC2 → PC1 en échec | Le pare-feu Windows bloque le ping entrant | Règle ICMP ajoutée avec `netsh` |
| 7 | PC1 / PC2 | SSH : « no matching key exchange method found », puis « no matching MAC found » | Le routeur ne propose que d'anciens algorithmes, refusés par défaut par les clients récents | Algorithmes autorisés dans la commande `ssh` |

---

## Bilan général

| Demandé | Résultat |
|---|---|
| Plan d'adressage et interfaces up/up | ✅ |
| Routage statique entre les sites | ✅ |
| Accès Internet par NAT | ✅ |
| Communication entre les deux postes | ✅ |
| Sécurité de base (enable secret, chiffrement, bannière, console) | ✅ |
| SSH et Telnet depuis les deux postes | ✅ |
| Démonstration Wireshark Telnet / SSH | ✅ |
| Configuration sauvegardée | ✅ |

## Fichiers joints

| Fichier | Contenu |
|---|---|
| R1.txt, R2.txt, R3.txt | show running-config des trois routeurs |
| Capture_Telnet.pcapng | Capture Wireshark de la session Telnet |
| Capture_SSH.pcapng | Capture Wireshark de la session SSH |