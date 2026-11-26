### Cahier des charges 3

# Compte rendu — Interconnexion réseau Windows 10 Pro et Ubuntu

**Technicien :** Negreche Sofia
**Environnement :** VMware Workstation
**Postes concernés :** Windows 10 Pro et Ubuntu
**Objectif :** Interconnecter les deux machines sur le même réseau local

---

# 1. Contexte

Les deux postes déployés, **Windows 10 Pro** et **Ubuntu**, doivent être interconnectés afin de permettre au client de les administrer et d'échanger des fichiers entre eux.

Les deux machines sont exécutées dans **VMware Workstation**. Elles doivent donc être configurées sur le même segment réseau afin de pouvoir communiquer directement.

L'objectif est de :

* configurer les deux machines sur le même réseau ;
* utiliser un adressage IP cohérent ;
* vérifier la connectivité entre les deux postes ;
* effectuer un ping dans les deux sens ;
* vérifier et diagnostiquer la connexion en cas de problème.

Le partage de fichiers avec SMB/Samba constitue une fonctionnalité optionnelle.

---

# 2. Configuration du réseau dans VMware Workstation

Dans VMware Workstation, les deux machines virtuelles ont été configurées afin d'utiliser le même réseau virtuel.

Le fait de placer les deux machines sur le même réseau permet aux systèmes Windows et Ubuntu de communiquer entre eux.

La configuration réseau utilisée doit permettre aux deux machines d'obtenir des adresses IP appartenant au même segment réseau.

L'adressage peut être réalisé avec une configuration **DHCP** ou avec des **adresses IP fixes**.

Dans notre configuration, les paramètres réseau des deux machines ont été vérifiés afin de s'assurer qu'ils sont cohérents.

---

# 3. Configuration réseau de Windows 10

Sur la machine Windows 10 Pro, nous avons vérifié la configuration réseau à l'aide de l'invite de commandes.

La commande utilisée est :

```cmd
ipconfig
```

Cette commande permet d'afficher les informations réseau de la machine.

Les informations importantes relevées sont :

* l'adresse IPv4 ;
* le masque de sous-réseau ;
* la passerelle par défaut.

### Configuration Windows

| Paramètre  | Valeur         |
| ---------- | -------------- |
| Machine    | Windows 10 Pro |
| Adresse IP | 192.168.31.128    |
| Masque     | 255.255.255.0    |


L'adresse IP de Windows appartient au même segment réseau que celle de la machine Ubuntu.

---

# 4. Configuration réseau d'Ubuntu

Sur la machine Ubuntu, nous avons utilisé la commande :

```bash
ip a
```

Cette commande permet d'afficher les interfaces réseau ainsi que les adresses IP configurées.

La configuration réseau de la machine Ubuntu a été vérifiée afin de s'assurer qu'elle se trouve sur le même segment réseau que Windows.

### Configuration Ubuntu

| Paramètre  | Valeur      |
| ---------- | ----------- |
| Machine    | Ubuntu      |
| Adresse IP | 192.168.31.129 |
| Masque     | 255.255.255.0|


Les paramètres réseau ont été configurés de manière cohérente avec ceux de Windows.

---

# 5. Tableau récapitulatif du réseau

| Paramètre     | Windows 10 Pro     | Ubuntu             |
| ------------- | ------------------ | ------------------ |
| Nom du poste  | PC01_SN            | PC02_SN            |
| Adresse IP    | 192.168.31.128|     192.168.31.129
| Masque        | 255.255.255.0        |255.255.255.0     |
| Réseau        | Même segment       | Même segment       |
| Environnement | VMware Workstation | VMware Workstation |

Les deux machines utilisent le même segment réseau, ce qui permet leur communication directe.

---

# 6. Test de connectivité Windows vers Ubuntu

Afin de vérifier que Windows peut communiquer avec Ubuntu, nous avons effectué un test `ping` depuis Windows.

La commande utilisée est :

```cmd
ping ADRESSE_IP_UBUNTU
```

L'adresse IP d'Ubuntu est remplacée par son adresse réelle.

Le test permet de vérifier que Windows peut atteindre la machine Ubuntu sur le réseau.

Un résultat contenant des réponses de la machine Ubuntu confirme que la communication fonctionne.

### Résultat

Le ping de **Windows vers Ubuntu** est réussi.

La communication réseau entre les deux machines est donc fonctionnelle dans ce sens.

---

# 7. Test de connectivité Ubuntu vers Windows

Nous avons ensuite effectué le test dans le sens inverse afin de vérifier que la communication est également possible depuis Ubuntu vers Windows.

La commande utilisée est :

```bash
ping ADRESSE_IP_WINDOWS
```

L'adresse IP de Windows est remplacée par son adresse réelle.

Des réponses reçues depuis Windows permettent de confirmer que la communication fonctionne également dans ce sens.

### Résultat

Le ping d'**Ubuntu vers Windows** est réussi.

La connectivité est donc **bidirectionnelle** entre les deux machines.

---

# 8. Diagnostic en cas de problème de connectivité

En cas d'échec d'un des tests `ping`, plusieurs éléments peuvent être vérifiés.

## 8.1 Vérification de l'adressage IP

La première étape consiste à vérifier les adresses IP des deux machines.

Sous Windows :

```cmd
ipconfig
```

Sous Ubuntu :

```bash
ip a
```

Les deux machines doivent avoir des adresses compatibles et appartenir au même segment réseau.

---

## 8.2 Vérification de VMware Workstation

La configuration réseau des deux machines virtuelles doit être vérifiée dans VMware Workstation.

Les deux machines doivent être connectées au réseau virtuel approprié.

Une mauvaise configuration de la carte réseau virtuelle peut empêcher les deux systèmes de communiquer.

---

## 8.3 Vérification du pare-feu Windows

Le pare-feu Windows peut bloquer les requêtes ICMP utilisées par la commande `ping`.

Si Windows ne répond pas au ping provenant d'Ubuntu, la configuration du pare-feu peut donc être vérifiée.

Il faut également vérifier que la connexion réseau de Windows est correctement configurée.

---

## 8.4 Vérification du pare-feu Ubuntu

Le pare-feu Ubuntu peut également être vérifié avec :

```bash
sudo ufw status
```

Une configuration trop restrictive du pare-feu peut empêcher certaines communications réseau.

---

## 8.5 Vérification de la connexion réseau

Enfin, il faut vérifier que les cartes réseau virtuelles des deux machines sont bien connectées et fonctionnelles.

Après correction d'un éventuel problème, les tests `ping` doivent être effectués à nouveau dans les deux sens.

---





# 9. Vérification finale

| Vérification                                | Résultat  |
| ------------------------------------------- | --------- |
| Windows connecté au réseau VMware           | ✅         |
| Ubuntu connecté au réseau VMware            | ✅         |
| Windows et Ubuntu sur le même segment       | ✅         |
| Adresse IP Windows vérifiée avec `ipconfig` | ✅         |
| Adresse IP Ubuntu vérifiée avec `ip a`      | ✅         |
| Ping Windows → Ubuntu                       | ✅         |
| Ping Ubuntu → Windows                       | ✅         |
| Connectivité bidirectionnelle               | ✅         |
| Pare-feu vérifié en cas de problème         | ✅         |
| Configuration VMware vérifiée               | ✅         |
| Partage SMB/Samba                           | Optionnel |

---

# 10. Conclusion

Les deux postes, **Windows 10 Pro et Ubuntu**, ont été configurés dans **VMware Workstation** afin de fonctionner sur le même segment réseau.

Les configurations IP des deux machines ont été vérifiées avec `ipconfig` sous Windows et `ip a` sous Ubuntu.

Des tests de connectivité ont ensuite été effectués dans les deux sens :

```text
Windows → Ubuntu
Ubuntu → Windows
```

Les tests `ping` permettent de confirmer la communication bidirectionnelle entre les deux machines.

En cas de problème de connectivité, les différents éléments pouvant être responsables ont été identifiés : configuration réseau de VMware, adressage IP, pare-feu et connexion des cartes réseau virtuelles.

Les deux postes sont ainsi configurés pour pouvoir communiquer sur le réseau local et peuvent être administrés depuis leur environnement réseau commun.
