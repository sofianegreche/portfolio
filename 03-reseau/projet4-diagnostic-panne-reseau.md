# Projet 4: Rapport d'intervention – Diagnostic de panne réseau

**Cours :** Networking Essentials – Projet 4
**Classe :** E1A – Geneva Institute of Technology
**Auteur :** Negreche Sofia
**Date :** 15 octobre 2026
**Outil :** GNS3

---

## 1. Contexte

Un utilisateur (PC1) signale qu'il n'a plus accès au réseau ni à Internet. L'objectif est de diagnostiquer la panne avec une méthode structurée (modèle OSI, approche *bottom-up*), de la corriger, puis de vérifier le retour à la normale.

Un second poste (PC2) sert de **poste témoin** : il permet de savoir si le problème touche tout le réseau ou seulement PC1.

## 2. Topologie et plan d'adressage

📷 **Image 1 – Topologie GNS3 avec les configurations**
![image](https://hackmd.io/_uploads/r1n3pB0jMx.png)

| Équipement | Interface | Adresse IP | Masque | Passerelle | Rôle |
|---|---|---|---|---|---|
| R1 (c7200) | Gi0/0 | 192.168.10.1 | 255.255.255.0 | – | Passerelle LAN (NAT inside) |
| R1 (c7200) | Gi1/0 | DHCP (192.168.208.141) | – | – | Sortie Internet (NAT outside) |
| PC1 (VPCS) | e0 | 192.168.10.11 | 255.255.255.0 | 192.168.10.1 | Poste utilisateur |
| PC2 (VPCS) | e0 | 192.168.10.12 | 255.255.255.0 | 192.168.10.1 | Poste témoin |
| Switch1 | – | – | – | – | Switch Ethernet GNS3 |
| NAT1 | nat0 | – | – | – | Accès Internet via l'hôte |

**Câblage du switch :**

| Port Switch1 | Équipement connecté |
|---|---|
| e0 | PC1 |
| e1 | PC2 |
| e2 | R1 Gi0/0 |

R1 fait du **NAT overload (PAT)** : les adresses du réseau 192.168.10.0/24 sont traduites vers l'adresse de Gi1/0 pour sortir sur Internet.

## 3. État de référence (avant panne)

Avant de provoquer la panne, on vérifie que tout fonctionne. Ces captures servent de point de comparaison.

### PC1

![image](https://hackmd.io/_uploads/H1XEAS0jfl.png)



- Configuration IP correcte (IP, masque, passerelle, DNS)
- Ping passerelle 192.168.10.1 : OK (TTL 255 = réponse directe du routeur)
- Ping PC2 : OK (TTL 64, temps très court = même réseau local)
- Ping 8.8.8.8 : OK (accès Internet via le NAT)

### PC2

![image](https://hackmd.io/_uploads/SkUeQ8CiGg.png)



Même résultat : PC2 joint la passerelle, PC1 et Internet.

### R1

![image](https://hackmd.io/_uploads/rJCTCSAizx.png)

- Gi0/0 et Gi1/0 sont en **up/up**
- Gi1/0 a bien reçu une adresse par DHCP (192.168.208.141)
- La table ARP contient PC1 (0050.7966.6800) et PC2 (0050.7966.6801) sur Gi0/0

![image](https://hackmd.io/_uploads/S1GJ1I0sfl.png)


- Gi1/0 en *outside*, Gi0/0 en *inside*
- 28 hits, 0 miss : le NAT traduit correctement les paquets

**Conclusion : le réseau fonctionne normalement.**

## 4. Simulation de la panne

La panne simulée est une **coupure du câble de PC1** : dans GNS3, le lien PC1 ↔ Switch1 est suspendu (clic droit → *Suspend*).

![image](https://hackmd.io/_uploads/BJnl1I0sGe.png)


## 5. Diagnostic

### 5.1 Symptôme côté utilisateur

![image](https://hackmd.io/_uploads/B1hE1U0szg.png)

PC1 ne joint plus sa passerelle. Le message *host not reachable* signifie que PC1 ne reçoit même pas de réponse ARP : il ne « voit » plus la passerelle.

### 5.2 Comparaison avec le poste témoin

![image](https://hackmd.io/_uploads/SkmUyIAsfg.png)

PC2 joint la passerelle sans problème. Le switch et le routeur fonctionnent : **le problème est isolé à PC1 ou à son lien**.

### 5.3 Méthode bottom-up (couche 1 → couche 3)

**Couche 1 – Physique**

Dans GNS3, le lien de PC1 est suspendu (symbole pause, points jaunes au lieu de verts). C'est l'équivalent d'un câble débranché ou d'une LED éteinte sur la carte réseau.

Côté routeur, on vide le cache ARP puis on essaie de joindre PC1 :

![image](https://hackmd.io/_uploads/S1vP1UAsfl.png)

![image](https://hackmd.io/_uploads/SkhdkLCozx.png)

Le ping échoue et 192.168.10.11 a disparu de la table ARP : R1 ne reçoit plus rien de PC1.

> Le `clear arp-cache` est important : sans lui, l'ancienne entrée ARP de PC1 serait restée affichée même avec le câble coupé, ce qui aurait faussé le diagnostic.

**Couche 3 – Réseau**

![image](https://hackmd.io/_uploads/ryRF1IRozg.png)

La configuration IP de PC1 est toujours correcte (192.168.10.11/24, passerelle 192.168.10.1). Un problème d'adressage est donc écarté.

### 5.4 Cause identifiée

**Panne de couche 1 : le lien physique entre PC1 et Switch1 est coupé.**

## 6. Correction

Le lien PC1 ↔ Switch1 est rétabli dans GNS3 (clic droit → *Resume*).

![image](https://hackmd.io/_uploads/rkfiJ8CjMg.png)


## 7. Vérification

![image](https://hackmd.io/_uploads/B1shkIRoGx.png)


![image](https://hackmd.io/_uploads/HkgexLRszx.png)


![image](https://hackmd.io/_uploads/S1H-ZURsMg.png)


- PC1 joint de nouveau la passerelle et Internet
- PC1 réapparaît dans la table ARP de R1 avec sa MAC 0050.7966.6800

> La latence vers 8.8.8.8 est plus élevée qu'avant la panne (300–400 ms au lieu d'environ 46 ms). Cela vient de la charge de la machine hôte et n'est pas lié à la panne.

**Conclusion : le service est rétabli.**

## 8. Limites du labo

La résolution DNS ne fonctionne pas depuis les VPCS (`Cannot resolve google.com`), même en essayant plusieurs serveurs DNS (8.8.8.8, 1.1.1.1, 192.168.208.2, 192.168.10.1).

![image](https://hackmd.io/_uploads/B1PXb80sMe.png)


Pourtant, la résolution fonctionne depuis R1 avec le DNS reçu par DHCP (192.168.208.2) :

![image](https://hackmd.io/_uploads/Bk3NWICofx.png)


L'accès Internet des PC est donc bien fonctionnel (ping 8.8.8.8 OK, NAT OK). Le problème vient de VPCS, qui gère mal le DNS dans GNS3. La commande `ip dns server` (pour faire de R1 un relais DNS) n'est pas disponible sur l'image IOS du c7200 utilisée.

## 9. Conclusion

La méthode bottom-up associée à un poste témoin a permis d'identifier rapidement la panne :

1. Le poste témoin a montré que le réseau fonctionnait et que seul PC1 était touché.
2. La vérification de la couche 1 (lien suspendu, PC1 absent de la table ARP de R1) a mis en évidence la coupure physique.
3. La vérification de la couche 3 (`show ip` correct) a confirmé qu'il ne s'agissait pas d'un problème d'adressage.

Après rétablissement du lien, la connectivité de PC1 a été entièrement vérifiée.

---

## Annexe – Commandes utilisées

### R1 (Cisco c7200)

```
enable
configure terminal
interface GigabitEthernet0/0
 description LAN - Reseau entreprise
 ip address 192.168.10.1 255.255.255.0
 ip nat inside
 no shutdown
 exit
interface GigabitEthernet1/0
 description WAN - Internet
 ip address dhcp
 ip nat outside
 no shutdown
 exit
access-list 1 permit 192.168.10.0 0.0.0.255
ip nat inside source list 1 interface GigabitEthernet1/0 overload
ip domain lookup
ip name-server 8.8.8.8
end
write memory
```

Vérification et diagnostic :

```
show ip interface brief
show arp
show ip nat translations
show ip nat statistics
clear arp-cache
ping 192.168.10.11
ping google.com
```

### PC1 (VPCS)

```
ip 192.168.10.11 255.255.255.0 192.168.10.1
ip dns 8.8.8.8
save
show ip
ping 192.168.10.1
ping 192.168.10.12
ping 8.8.8.8
ping google.com
```

### PC2 (VPCS)

```
ip 192.168.10.12 255.255.255.0 192.168.10.1
ip dns 8.8.8.8
save
show ip
ping 192.168.10.1
ping 192.168.10.11
ping 8.8.8.8
```

### GNS3

| Action | Méthode |
|---|---|
| Provoquer la panne | Clic droit sur le lien PC1 ↔ Switch1 → *Suspend* |
| Corriger la panne | Clic droit sur le lien PC1 ↔ Switch1 → *Resume* |