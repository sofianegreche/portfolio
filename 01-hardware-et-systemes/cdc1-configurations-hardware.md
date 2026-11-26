# Cahier des charges 1 — Conception de configurations harware

PC GAMER

| Catégorie | Marque / Modèle | Prix |
|---|---|---:|
| Processeur (CPU) | AMD Ryzen 5 7600 | CHF 150.– |
| Refroidissement CPU | Thermalright Peerless Assassin 120 SE | CHF 45.– |
| Carte mère | MSI B650 Gaming Plus WiFi | CHF 155.– |
| Mémoire vive (RAM) | Corsair Vengeance 32 Go (2×16 Go) DDR5-6000 CL30 | CHF 105.– |
| Carte graphique (GPU) | Sapphire Radeon RX 7900 GRE 16 Go | CHF 580.– |
| Stockage | WD Black SN850X 1 To NVMe PCIe 4.0 | CHF 100.– |
| Boîtier | Montech AIR 903 MAX | CHF 100.– |
| Alimentation (PSU) | be quiet! Pure Power 12 M 750 W 80+ Gold | CHF 115.– |
| **TOTAL** | | **CHF 1'350.–** |



## 3. Vérifications de compatibilité

### Vérification 1 — Socket CPU / carte mère

- **Processeur :** AMD Ryzen 5 7600
- **Socket CPU :** AM5
- **Carte mère :** MSI B650 Gaming Plus WiFi
- **Socket carte mère :** AM5

**Résultat :**  
Le Ryzen 5 7600 et la carte mère utilisent tous les deux le socket **AM5**.

→ **COMPATIBLE **

---

### Vérification 2 — RAM / carte mère

- **Carte mère :** MSI B650 Gaming Plus WiFi
- **Type de RAM accepté :** DDR5
- **RAM choisie :** 32 Go (2×16 Go) DDR5-6000 CL30

La carte mère B650 est compatible avec la mémoire **DDR5**.  
Les deux barrettes de 16 Go permettent également d'utiliser le **Dual Channel**, ce qui améliore les performances mémoire.

→ **COMPATIBLE **

---

### Vérification 3 — TDP du CPU / puissance de l'alimentation

- **CPU :** AMD Ryzen 5 7600
- **TDP du CPU :** 65 W
- **Alimentation :** be quiet! Pure Power 12 M
- **Puissance de l'alimentation :** 750 W

**Calcul :**

750 W − 65 W = **685 W de marge théorique**

Le processeur consomme donc une faible partie de la puissance disponible.  
Même en prenant en compte la consommation de la carte graphique RX 7900 GRE et des autres composants, une alimentation de **750 W** est adaptée à cette configuration.

→ **COMPATIBLE **

---

### Résumé des vérifications

| Vérification | Résultat |
|---|---|
| CPU / Carte mère — AM5 |  Compatible |
| RAM / Carte mère — DDR5 |  Compatible |
| CPU / PSU — 65 W vs 750 W |  Compatible |

|
## Justification des composants principaux

CPU — AMD Ryzen 5 7600
Le Ryzen 5 7600 possède 6 cœurs et 12 threads, ce qui est largement suffisant pour accompagner une carte graphique performante dans les jeux en 1440p. Son TDP de 65 W permet également de conserver une consommation et des températures raisonnables.

GPU — Sapphire Radeon RX 7900 GRE 16 Go
La RX 7900 GRE est le composant prioritaire pour cet ordinateur, car le client souhaite jouer aux jeux AAA récents en 1440p à plus de 60 FPS. Ses 16 Go de VRAM offrent une bonne réserve pour les textures et les jeux modernes.

RAM — 32 Go DDR5-6000 CL30
32 Go permettent de jouer confortablement aux jeux récents tout en gardant d'autres applications ouvertes. La DDR5-6000 avec un kit 2×16 Go est particulièrement adaptée à la plateforme Ryzen AM5.

Carte mère — MSI B650 Gaming Plus WiFi
Cette carte mère utilise le socket AM5, compatible avec le Ryzen 5 7600, et prend en charge la DDR5. Elle offre également une bonne base pour faire évoluer la configuration ultérieurement.

Refroidissement — Thermalright Peerless Assassin 120 SE
Ce ventirad double tour assure un refroidissement efficace et silencieux du Ryzen 5 7600. Il apporte également une bonne marge thermique lors des longues sessions de jeu.

Boîtier — Montech AIR 903 MAX
Le boîtier a été choisi pour son bon airflow, indispensable avec une carte graphique puissante comme la RX 7900 GRE. Son format ATX offre suffisamment d'espace pour les différents composants.

Alimentation — be quiet! Pure Power 12 M 750 W
Une alimentation de 750 W offre une marge confortable pour le Ryzen 5 7600 et la RX 7900 GRE. La certification 80 PLUS Gold dépasse la contrainte minimale de 80 PLUS Bronze.

PC Administration

## PC Administrateur — Configuration finale

| Catégorie | Marque / Modèle | Prix |
|---|---|---:|
| Processeur (CPU) | AMD Ryzen 5 5600G — 6 cœurs / 12 threads | CHF 115.– |
| Refroidissement CPU | AMD Wraith Stealth | CHF 0.– |
| Carte mère | MSI B550M PRO-VDH | CHF 85.– |
| Mémoire vive (RAM) | 32 Go (2×16 Go) DDR4-3200 | CHF 80.– |
| Stockage | Samsung 990 EVO 2 To NVMe | CHF 130.– |
| Carte graphique (GPU) | Radeon Vega 7 intégrée au Ryzen 5 5600G | CHF 0.– |
| Boîtier | Cooler Master MasterBox Q300L | CHF 55.– |
| Alimentation (PSU) | be quiet! System Power 10 450 W 80+ Bronze | CHF 60.– |
| | Ventilateur supplémentaire | be quiet! Pure Wings 2 120 mm | CHF 20.– |
| **TOTAL FINAL** | | **CHF 545.–** |



| ## Vérifications de compatibilité

### Vérification 1 — Socket CPU / carte mère

- **Processeur :** AMD Ryzen 5 5600G
- **Socket :** AM4
- **Carte mère :** MSI B550M PRO-VDH
- **Socket carte mère :** AM4

Le Ryzen 5 5600G utilise le socket AM4 et la carte mère MSI B550M PRO-VDH utilise également le socket AM4.

→ **COMPATIBLE **

---

### Vérification 2 — RAM / carte mère

- **Carte mère :** MSI B550M PRO-VDH
- **Type de mémoire :** DDR4
- **RAM choisie :** 32 Go (2×16 Go) DDR4-3200

La carte mère B550M accepte la mémoire DDR4. 
Les deux barrettes de 16 Go permettent également de profiter du Dual Channel.

→ **COMPATIBLE *

---

### Vérification 3 — TDP du CPU / puissance de l'alimentation

- **CPU :** AMD Ryzen 5 5600G
- **TDP du CPU :** 65 W
- **Alimentation :** be quiet! System Power 10
- **Puissance :** 450 W

### Calcul

450 W − 65 W = **385 W de marge théorique**

Le Ryzen 5 5600G ne consomme qu'une partie de la puissance disponible. 
Comme la configuration ne possède pas de carte graphique dédiée, une alimentation de 450 W est largement suffisante.

→ **COMPATIBLE 

## Justification des composants

### Processeur — AMD Ryzen 5 5600G

Le Ryzen 5 5600G possède 6 cœurs et 12 threads, ce qui est largement suffisant pour une utilisation bureautique avec Office, un ERP web et plusieurs fenêtres ouvertes simultanément. Son processeur graphique Radeon Vega 7 intégré permet de ne pas utiliser de carte graphique dédiée.

### Mémoire vive — 32 Go DDR4

32 Go de RAM permettent de travailler confortablement avec plusieurs applications ouvertes en même temps, notamment Word, Excel, navigateur web et ERP. Le kit 2×16 Go permet également de profiter du Dual Channel.

### Stockage — Samsung 990 EVO 2 To

Le SSD NVMe permet un démarrage rapide de Windows ainsi qu'une bonne réactivité lors de l'ouverture des applications et des fichiers. Une capacité de 2 To offre également beaucoup d'espace pour les documents professionnels et les logiciels.

### Carte graphique — Radeon Vega 7 intégrée

Une carte graphique dédiée n'est pas nécessaire pour un poste administratif. Le GPU intégré au Ryzen 5 5600G suffit pour l'affichage, la bureautique et les applications web tout en réduisant la consommation et le coût de la machine.

### Boîtier — Cooler Master MasterBox Q300L

Le format micro-ATX permet de réaliser une configuration compacte, ce qui est intéressant pour un bureau de PME. Le boîtier offre également suffisamment d'espace pour les composants choisis.

### Alimentation — be quiet! System Power 10 450 W

Une alimentation de 450 W est largement suffisante pour cette configuration qui ne possède pas de carte graphique dédiée. La certification 80 PLUS Bronze respecte également les exigences de qualité et d'efficacité énergétique.


## PC Étudiant — Informatique

| Catégorie | Marque / Modèle | Prix |
|---|---|---:|
| Processeur (CPU) | AMD Ryzen 7 7700 — 8 cœurs / 16 threads | CHF 250.– |
| Refroidissement CPU | AMD Wraith Prism | CHF 0.– |
| Carte mère | MSI PRO B650-S WIFI | CHF 135.– |
| Mémoire vive (RAM) | 32 Go (2×16 Go) DDR5-6000 | CHF 100.– |
| Carte graphique (GPU) | AMD Radeon RX 7600 8 Go | CHF 250.– |
| Stockage | WD Black SN770 1 To NVMe PCIe 4.0 | CHF 85.– |
| Boîtier | Montech AIR 100 ARGB | CHF 65.– |
| Alimentation (PSU) | be quiet! System Power 10 650 W 80+ Bronze | CHF 75.– |
| **TOTAL** | | **CHF 960.–** |

## Vérifications de compatibilité

### Vérification 1 — Socket CPU / carte mère

- **Processeur :** AMD Ryzen 7 7700
- **Socket :** AM5
- **Carte mère :** MSI PRO B650-S WIFI
- **Socket carte mère :** AM5

Le Ryzen 7 7700 utilise le socket AM5 et la carte mère B650 est conçue pour les processeurs AM5.

→ **COMPATIBLE **

---

### Vérification 2 — RAM / carte mère

- **Carte mère :** MSI PRO B650-S WIFI
- **Type de RAM :** DDR5
- **RAM choisie :** 32 Go (2×16 Go) DDR5-6000

La carte mère B650 accepte la mémoire DDR5. 
Le kit composé de deux barrettes de 16 Go permet d'utiliser le Dual Channel et les emplacements supplémentaires de la carte mère permettent une future extension.

→ **COMPATIBLE **

---

### Vérification 3 — TDP du CPU / puissance de l'alimentation

- **CPU :** AMD Ryzen 7 7700
- **TDP du CPU :** 65 W
- **Alimentation :** be quiet! System Power 10
- **Puissance :** 650 W

### Calcul

650 W − 65 W = **585 W de marge théorique**

Le processeur utilise donc une faible partie de la puissance maximale de l'alimentation. Même en ajoutant la consommation de la Radeon RX 7600 et des autres composants, 650 W fournit une marge suffisante.

→ **COMPATIBLE **

## Justification des composants principaux

### Processeur — AMD Ryzen 7 7700

Le Ryzen 7 7700 possède 8 cœurs et 16 threads, ce qui est particulièrement adapté à la virtualisation avec plusieurs machines virtuelles fonctionnant simultanément. Il offre également suffisamment de performances pour le développement, la compilation et la bureautique.

### RAM — 32 Go DDR5

32 Go de RAM sont importants pour la virtualisation, car chaque machine virtuelle peut avoir besoin de plusieurs gigaoctets de mémoire. Le choix de 2×16 Go permet également d'utiliser le Dual Channel et laisse la possibilité d'augmenter la capacité plus tard.

### Carte mère — MSI PRO B650-S WIFI

La carte mère B650 est compatible avec le Ryzen 7 7700 et utilise la mémoire DDR5. Elle offre plusieurs emplacements et connecteurs permettant de faire évoluer la configuration, notamment pour ajouter de la RAM ou du stockage.

### Carte graphique — Radeon RX 7600 8 Go

Une carte graphique dédiée n'est pas indispensable pour les études d'informatique, mais elle permet de jouer occasionnellement dans de bonnes conditions en 1080p. La RX 7600 offre donc un bon compromis entre performances graphiques et budget.

### SSD — WD Black SN770 1 To

Le SSD NVMe permet de démarrer rapidement le système et les logiciels de développement. 1 To est une capacité intéressante pour stocker les projets, logiciels et plusieurs machines virtuelles, même qu'un second SSD pourrait être ajouté ultérieurement.

### Alimentation — be quiet! System Power 10 650 W

Une alimentation de 650 W offre suffisamment de puissance pour le Ryzen 7 7700 et la RX 7600 tout en conservant une marge pour les autres composants. La certification 80 PLUS Bronze respecte également les exigences d'efficacité énergétique.

