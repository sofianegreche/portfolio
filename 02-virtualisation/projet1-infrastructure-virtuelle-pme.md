# Compte rendu – Projet 1 : Infrastructure virtuelle PME

7 octobre 2026 · Negreche Sofia · E1A

## Contexte et objectifs

Une PME de 15 salariés veut virtualiser ses 3 serveurs physiques (fichiers, impression, applicatif) pour réduire ses coûts matériels. Ce compte rendu décrit l'infrastructure mise en place et validée dans le cadre du PPE d'une semaine du cours Virtualisation (classe E1A).

Objectifs du projet :

- Installer un hyperviseur et créer 3 machines virtuelles avec des rôles distincts.
- Configurer le réseau virtuel pour que les VM communiquent entre elles et avec l'extérieur.
- Allouer les ressources (CPU, RAM, disque) de façon cohérente.
- Valider le fonctionnement et documenter l'architecture.

**Résultat : les 4 objectifs sont atteints.** Les 3 serveurs fonctionnent et chaque service a été testé depuis une autre VM.

## Architecture choisie

L'infrastructure tourne sur **VMware Workstation**, installé sur le PC hôte. Les 3 VM sont reliées au réseau virtuel **NAT (VMnet8)** en 192.168.208.0/24. Elles communiquent directement entre elles et sortent sur Internet par la passerelle NAT de VMware (192.168.208.2).

![01_schema_architecture](https://hackmd.io/_uploads/rkrr_GUjGe.png)

| VM | OS | Rôle | Adresse IP | Service |
| --- | --- | --- | --- | --- |
| SVR-FICHIERS | Windows 11 | Serveur de fichiers | 192.168.208.11 (fixe) | Partage SMB « Partage » (port 445) |
| SVR-APPLI | Ubuntu 26.04 LTS | Serveur applicatif | 192.168.208.12 (fixe) | Serveur web Apache (port 80) |
| SVR-IMPRESSION | Windows 11 | Serveur d'impression | 192.168.208.13 (fixe) | Imprimante partagée IMP-PME |
| Passerelle / DNS | NAT VMware | Sortie Internet | 192.168.208.2 | – |

Masque utilisé : 255.255.255.0. Les IP fixes ont été choisies hors de la plage DHCP de VMware (.128 à .254) pour éviter tout conflit.

Remarque : Windows 11 n'est pas un OS serveur. Il limite le partage à 20 connexions simultanées, ce qui suffit pour 15 salariés. En production, on utiliserait Windows Server.

## Préparation : VMware Tools

Les VMware Tools ont été installés sur les VM Windows pour activer le copier-coller entre le PC hôte et les VM. Ils améliorent aussi l'affichage et les performances.

1. Menu VMware **VM → Install VMware Tools** : un lecteur DVD virtuel « VMware Tools » apparaît dans la VM.
2. Lancement de **setup.exe**, installation « Typique », puis redémarrage de la VM.
3. Vérification dans **VM → Settings → Options → Isolation de l'invité** : « Activer le copier-coller » est coché.

![image](https://hackmd.io/_uploads/S1gmtfIsGg.png)

![image](https://hackmd.io/_uploads/SJ4rtfIiMe.png)

Sur Ubuntu, l'équivalent est le paquet `open-vm-tools-desktop`. Dans le terminal Ubuntu, le collage se fait avec Ctrl+Shift+V.

## Configuration réseau et tests de communication

Les 3 VM communiquent entre elles : tous les pings répondent à 100 %, en moins de 3 ms.

Sur les VM Windows, le réseau a été passé en profil « Privé » et le pare-feu ouvert pour le partage et le ping :

```powershell
Set-NetConnectionProfile -NetworkCategory Private
Enable-NetFirewallRule -Group "@FirewallAPI.dll,-28502"
```

SVR-FICHIERS recevait d'abord son adresse par DHCP (192.168.208.131). Elle a été passée en IP fixe 192.168.208.11 via Paramètres → Réseau et Internet → Ethernet → Attribution d'adresse IP → Manuel. SVR-IMPRESSION a reçu l'IP fixe 192.168.208.13 en PowerShell :

```powershell
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 192.168.208.13 -PrefixLength 24 -DefaultGateway 192.168.208.2
Set-DnsClientServerAddress -InterfaceAlias "Ethernet0" -ServerAddresses 192.168.208.2
```

| Test | Depuis | Vers | Résultat |
| --- | --- | --- | --- |
| Ping | SVR-APPLI (.12) | SVR-FICHIERS (.11) | 12/12 reçus, 0 % de perte |
| Ping | SVR-FICHIERS (.11) | SVR-IMPRESSION (.13) | 4/4 reçus, 0 % de perte |
| Ping | SVR-APPLI (.12) | SVR-IMPRESSION (.13) | 4/4 reçus, 0 % de perte |

![image](https://hackmd.io/_uploads/r1IKtfIoGg.png)

![image](https://hackmd.io/_uploads/S129tz8jfg.png)

## Serveur de fichiers : SVR-FICHIERS

Le serveur de fichiers est opérationnel : un dossier créé depuis SVR-APPLI (Ubuntu) apparaît bien dans `C:\Partage` sur SVR-FICHIERS.

Mise en place (PowerShell administrateur) :

```powershell
New-Item -Path C:\Partage -ItemType Directory
net user employe <mot_de_passe> /add
New-SmbShare -Name "Partage" -Path "C:\Partage" -FullAccess "employe"
```

Les droits NTFS ont ensuite été donnés au compte `employe` sur le dossier : clic droit sur `C:\Partage` → Propriétés → Sécurité → Modifier → Ajouter `employe` → droit **Modification**.

**Test depuis SVR-APPLI (Ubuntu)** : dans Fichiers, Ctrl+L puis `smb://192.168.208.131/`, connexion avec le compte `employe`. Les partages de SVR-FICHIERS s'affichent (ADMIN$ et C$ sont des partages d'administration créés automatiquement par Windows).

![image](https://hackmd.io/_uploads/ByS-5z8szx.png)

Dans le partage, un dossier `test-ubuntu` a été créé depuis Ubuntu. Il apparaît côté Windows :

![image](https://hackmd.io/_uploads/ByGEqf8ofl.png)

Ce test a été réalisé quand SVR-FICHIERS était encore en 192.168.208.131. Depuis le passage en IP fixe, l'adresse du partage est `smb://192.168.208.11/Partage`.

## Serveur d'impression : SVR-IMPRESSION

L'imprimante IMP-PME est créée, partagée et validée : une impression envoyée depuis SVR-APPLI (Ubuntu) arrive bien dans `C:\Impressions\impression.txt`.

Faute d'imprimante physique, IMP-PME est une imprimante virtuelle : chaque impression est enregistrée dans le fichier `C:\Impressions\impression.txt`, ce qui sert de preuve de fonctionnement.

```powershell
New-Item -Path C:\Impressions -ItemType Directory
Add-PrinterPort -Name "C:\Impressions\impression.txt"
Restart-Service Spooler
Add-PrinterDriver -Name "Generic / Text Only"
Add-Printer -Name "IMP-PME" -DriverName "Generic / Text Only" -PortName "C:\Impressions\impression.txt"
Set-Printer -Name "IMP-PME" -Shared $true -ShareName "IMP-PME"
```

Le pilote « Microsoft Print To PDF » a d'abord été essayé, mais Windows 11 refuse de le partager (erreur 0x80070bce). Il a été remplacé par le pilote « Generic / Text Only », qui est partageable. Un compte `employe` a aussi été créé sur SVR-IMPRESSION pour l'accès des clients.

![image](https://hackmd.io/_uploads/SkZLcf8jGx.png)

**Test depuis SVR-APPLI (Ubuntu)** : installation de l'outil `smbclient`, création d'un fichier texte, puis envoi à l'imprimante partagée.

```bash
sudo apt install smbclient -y
echo "Test impression depuis SVR-APPLI" > test.txt
smbclient //192.168.208.13/IMP-PME -U employe%<mot_de_passe> -c "print test.txt"
```

Le message `putting file test.txt as test.txt` confirme l'envoi.

![09_impression_smbclient_ubuntu](https://hackmd.io/_uploads/B1299f8oGe.png)

Côté Windows, le texte envoyé est bien présent dans le fichier de sortie :

```powershell
Get-Content C:\Impressions\impression.txt
```

![10_impression_txt_windows](https://hackmd.io/_uploads/ByjhczIjMe.png)

## Serveur applicatif : SVR-APPLI

Le serveur applicatif est opérationnel : la page web hébergée sur SVR-APPLI s'affiche depuis une VM Windows.

```bash
sudo apt install apache2 -y
systemctl status apache2 --no-pager
echo "<h1>SVR-APPLI - Serveur applicatif PME</h1><p>Projet 1 - Negreche Sofia - E1A</p>" | sudo tee /var/www/html/index.html
```

Le service est **active (running)** et **enabled** : il redémarre automatiquement avec la VM. Le message `AH00558` dans les logs est un simple avertissement (nom de serveur non défini) et n'empêche pas le fonctionnement.

![11_apache_actif](https://hackmd.io/_uploads/SyHkjzIofl.png)

**Test depuis SVR-IMPRESSION** : navigateur ouvert sur `http://192.168.208.12`.

![12_page_web_depuis_svr-impression](https://hackmd.io/_uploads/rk8-sGLsGl.png)

Le navigateur affiche « Non sécurisé » car le site est en HTTP (port 80). En production, on ajouterait un certificat pour passer en HTTPS (port 443).

## Allocation des ressources : prévu et réel

Les ressources réelles ont été vérifiées dans chaque VM. Les 2 VM Windows sont conformes au plan ; SVR-APPLI a reçu plus que prévu.

Commandes utilisées :

- Ubuntu : `nproc` (vCPU), `free -h` (RAM), `lsblk` (disque).
- Windows (PowerShell) : `Get-CimInstance Win32_ComputerSystem | Select NumberOfLogicalProcessors, TotalPhysicalMemory` et `Get-PSDrive C`.

| VM | vCPU | RAM | Disque | Justification |
| --- | --- | --- | --- | --- |
| SVR-FICHIERS | 2 | 4 Go | 64 Go | Minimum exigé par Windows 11 ; stockage des fichiers des salariés |
| SVR-IMPRESSION | 2 | 4 Go | 64 Go | Minimum exigé par Windows 11 ; service peu gourmand |
| SVR-APPLI | 4 | 8 Go | 50 Go | VM réutilisée d'un TP précédent, volontairement large ; 2 vCPU et 2 à 4 Go suffiraient pour Apache en production |

![image](https://hackmd.io/_uploads/SJ_7sz8jfe.png)

![image](https://hackmd.io/_uploads/HJsHof8izg.png)

![image](https://hackmd.io/_uploads/BkrOjzLofg.png)

![image](https://hackmd.io/_uploads/Hykioz8iGe.png)

Remarques :

- Windows affiche environ 63 Go (Used + Free) au lieu de 64 Go : une partie du disque est réservée au démarrage et à la récupération, et Windows compte en Gio.
- Windows 11 occupe déjà 32 Go, soit la moitié du disque de chaque VM Windows. Windows Server Core serait beaucoup plus léger en production.
- SVR-APPLI n'utilise que 1,4 Go de RAM en fonctionnement. On pourrait la réduire à 2 vCPU / 4 Go (VM éteinte, VM → Settings) sans perte de performance. Le disque est alloué dynamiquement : il n'occupe pas réellement 50 Go sur le PC hôte.

## Difficultés rencontrées et solutions

Huit problèmes ont été rencontrés ; sept sont résolus, le dernier (authentification entre VM clonées) est contourné.

| Problème | Cause | Solution |
| --- | --- | --- |
| Copier-coller impossible dans les VM | VMware Tools absents | Installation des VMware Tools, option copier-coller activée |
| Erreurs en appliquant les droits NTFS | Droits appliqués par erreur sur tout le disque C: au lieu du dossier | Annulation, retrait du compte sur C:, droits remis sur `C:\Partage` uniquement |
| SVR-FICHIERS injoignable en cours de test | IP attribuée par DHCP, puis VM éteinte | IP fixe 192.168.208.11 ; vérifier que les 3 VM sont allumées avant chaque test |
| Imprimante non partageable (0x80070bce) | Pilote Microsoft Print To PDF bloqué au partage | Pilote Generic / Text Only |
| Commandes déformées (`\` devient `$`, `y` devient `z`) | Texte injecté comme un clavier QWERTY dans une VM en QWERTZ | Saisie manuelle ou passage par l'interface graphique |
| smbclient : NT_STATUS_HOST_UNREACHABLE | SVR-IMPRESSION pas encore démarrée au moment du test | Ping de vérification, puis nouvelle tentative une fois la VM prête |
| Ressources de SVR-APPLI supérieures au plan | VM réutilisée d'un TP précédent | Écart constaté et justifié ; réductible à 2 vCPU / 4 Go |
| Mot de passe refusé entre SVR-FICHIERS et SVR-IMPRESSION | Les 2 VM Windows sont des clones : même nom (DESKTOP-R9Q2IIA) et même SID | Renommage des 2 VM ; test d'impression réalisé depuis SVR-APPLI (Ubuntu) : OK |

![image](https://hackmd.io/_uploads/ByL0izIofx.png)

![image](https://hackmd.io/_uploads/S1Lx2fIoze.png)

![image](https://hackmd.io/_uploads/HyMG3MLjfg.png)

Le dernier problème est le plus instructif. Les deux VM Windows portaient le même nom de machine :

![image](https://hackmd.io/_uploads/HJEkTfIsGg.png)

Après renommage en SVR-FICHIERS et SVR-IMPRESSION, l'authentification reste refusée. Le clonage conserve aussi l'identifiant de sécurité (SID), et les versions récentes de Windows 11 bloquent l'authentification entre deux machines au même SID.

![image](https://hackmd.io/_uploads/r1HZ6MUozl.png)

En production, chaque VM clonée doit être préparée avec **Sysprep** (qui génère un nouveau SID), ou installée séparément. Ici, le problème a été contourné en testant les services depuis SVR-APPLI, qui n'est pas un clone.

## Bilan

Les 3 serveurs sont opérationnels et testés depuis une autre VM du réseau.

| Élément | État |
| --- | --- |
| Hyperviseur VMware Workstation + 3 VM | OK |
| Réseau NAT et IP fixes (.11, .12, .13) | OK, pings validés |
| Serveur de fichiers SVR-FICHIERS | OK, test depuis Ubuntu |
| Serveur d'impression SVR-IMPRESSION | OK, impression depuis SVR-APPLI |
| Serveur applicatif SVR-APPLI | OK, page web vue depuis Windows |
| Fiches de configuration | OK (annexe) |

## Conclusion

Ce projet montre qu'une PME peut regrouper ses 3 serveurs sur une seule machine physique grâce à la virtualisation. Il m'a permis de pratiquer la configuration réseau (IP fixes, NAT, pare-feu), le partage SMB entre Windows et Linux, et la vérification des ressources allouées.

Pour un passage en production, il faudrait :

- utiliser Windows Server à la place de Windows 11 ;
- préparer les VM clonées avec Sysprep ;
- passer le serveur web en HTTPS ;
- ajuster les ressources de SVR-APPLI ;
- prévoir des snapshots et des sauvegardes régulières.

## Annexe : fiches de configuration

Une fiche par VM.

### Fiche 1 : SVR-FICHIERS

| Élément | Valeur |
| --- | --- |
| Rôle | Serveur de fichiers |
| Système | Windows 11 x64 |
| Nom de machine | SVR-FICHIERS (anciennement DESKTOP-R9Q2IIA) |
| vCPU / RAM / Disque | 2 / 4 Go / 64 Go |
| Carte réseau | NAT (VMnet8), Ethernet0 |
| Adresse IP | 192.168.208.11 (fixe) |
| Masque | 255.255.255.0 |
| Passerelle / DNS | 192.168.208.2 |
| Service | Partage SMB « Partage » → `C:\Partage` (port 445) |
| Compte d'accès | `employe` (droits partage : contrôle total ; NTFS : modification) |
| Pare-feu | Profil Privé, groupe « Partage de fichiers et d'imprimantes » activé |
| Test de validation | Dossier `test-ubuntu` créé depuis SVR-APPLI, visible dans `C:\Partage` : OK |

### Fiche 2 : SVR-IMPRESSION

| Élément | Valeur |
| --- | --- |
| Rôle | Serveur d'impression |
| Système | Windows 11 x64 |
| Nom de machine | SVR-IMPRESSION (anciennement DESKTOP-R9Q2IIA) |
| vCPU / RAM / Disque | 2 / 4 Go / 64 Go |
| Carte réseau | NAT (VMnet8), Ethernet0 |
| Adresse IP | 192.168.208.13 (fixe) |
| Masque | 255.255.255.0 |
| Passerelle / DNS | 192.168.208.2 |
| Service | Imprimante IMP-PME, partagée sous le nom IMP-PME (port 445) |
| Pilote / port | Generic / Text Only → fichier `C:\Impressions\impression.txt` |
| Compte d'accès | `employe` |
| Pare-feu | Profil Privé, groupe « Partage de fichiers et d'imprimantes » activé |
| Test de validation | Imprimante partagée (Shared = True) : OK ; impression depuis SVR-APPLI : OK |

### Fiche 3 : SVR-APPLI

| Élément | Valeur |
| --- | --- |
| Rôle | Serveur applicatif |
| Système | Ubuntu 26.04 LTS |
| Nom de machine | administrator-VMware-Virtual-Platform |
| vCPU / RAM / Disque | 4 / 8 Go / 50 Go |
| Carte réseau | NAT (VMnet8) |
| Adresse IP | 192.168.208.12 (fixe) |
| Masque | 255.255.255.0 |
| Passerelle / DNS | 192.168.208.2 |
| Service | Serveur web Apache (port 80), actif |
| Compte | `administrator` |
| Outils | open-vm-tools, smbclient (pour le test d'impression) |
| Test de validation | Ping vers SVR-FICHIERS : OK ; accès au partage : OK ; page web depuis SVR-IMPRESSION : OK |

### Captures complémentaires

Captures des paramètres et des tests de chaque VM :

![image](https://hackmd.io/_uploads/HyUhkQ8iGe.png)

![image](https://hackmd.io/_uploads/SyrTlQUoGe.png)

![image](https://hackmd.io/_uploads/H1B1-Q8ofe.png)

![image](https://hackmd.io/_uploads/S1CsbQ8oGl.png)

![image](https://hackmd.io/_uploads/Sk3mmm8jzl.png)

![image](https://hackmd.io/_uploads/S1EFmX8oMg.png)

![image](https://hackmd.io/_uploads/Sk6EHmUsMe.png)

![image](https://hackmd.io/_uploads/BykIHQUiMe.png)

![image](https://hackmd.io/_uploads/rkRLSXUoMg.png)