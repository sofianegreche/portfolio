# Projet 5 — Comparatif Code vs No-Code

**Cours :** HTML - No Code · **Classe :** E1A · **Niveau :** Avancé · **Durée :** 1 semaine (PPE)
**Élève :** Sofia Negreche · **Date :** 06/11/2026

---

## 1. Contexte

Un client hésite entre faire coder son site sur-mesure ou utiliser un outil No-Code. Pour lui répondre, j'ai construit **la même page** avec les deux approches, puis je les ai comparées sur des critères objectifs.

**Cas client retenu :** éléctri'tek, petite entreprise d'électricien à Genève, qui veut une page pour vendre l'installation de **bornes de recharge pour voiture électrique** à domicile.

**La page construite (identique dans les deux versions) :**
- en-tête avec logo et menu
- accroche « Rechargez chez vous. Partez toujours à 100 % » + bouton « Tester mon éligibilité »
- 3 avantages : recharge ultra-rapide, installation certifiée, gestion via application
- présentation de l'entreprise (Expertise Éléctri'tek)
- 3 témoignages clients
- FAQ
- formulaire de contact
- pied de page

---

## 2. Version No-Code (Wix)

Lien : https://negrechesofia.wixsite.com/my-site-1

J'ai utilisé un modèle Wix que j'ai personnalisé : textes, menu, bouton d'appel à l'action et sections.

![01_wix_desktop_accueil](https://hackmd.io/_uploads/S1yEFOiTMg.jpg)


![02_wix_desktop_avantages](https://hackmd.io/_uploads/Sk-HKuoTzl.jpg)


![03_wix_desktop_expertise_temoignages](https://hackmd.io/_uploads/SJEUKOi6Gg.jpg)


**Observation :** la FAQ contient encore le texte du modèle Wix (« Qu'est-ce qu'une section FAQ ? »). Chaque bloc du modèle doit être modifié à la main, et on l'oublie facilement.

![04_wix_desktop_faq_texte_modele](https://hackmd.io/_uploads/B1BPtui6Gl.jpg)


**Observation :** le pied de page affiche aussi les coordonnées du modèle (info@monsite.com, adresse américaine) et la mention « Propulsé et sécurisé par Wix ». Le bandeau « Créé avec Wix » reste en haut de la page avec le forfait gratuit.

![05_wix_desktop_pied_de_page_modele](https://hackmd.io/_uploads/BJnqtds6Mg.jpg)

**Observation :** sur mobile, Wix adapte la page automatiquement, mais le texte sous le titre devient minuscule et presque illisible.

![06_wix_mobile_accueil](https://hackmd.io/_uploads/r1J3KuopMg.jpg)


---

## 3. Version codée (HTML/CSS pur)

Fichiers : `projet5.html` et `projet5.css` (aucune bibliothèque, aucun framework).

Choix techniques :
- **HTML sémantique** : `header`, `nav`, `main`, `section`, `article`, `blockquote`, `footer`
- **CSS Grid** pour l'accroche et les avantages (une colonne sur deux est inversée pour alterner texte / illustration)
- **Variables CSS** (`--accent`, `--sombre`…) pour changer toutes les couleurs en une seule ligne
- **FAQ avec `<details>` / `<summary>`** : s'ouvre et se ferme sans JavaScript
- **Formulaire accessible** : chaque champ a un `label`, validation HTML5 (`required`, `type="email"`), message de confirmation affiché sur la page
- **Responsive** : une media query à 800 px passe tout sur une colonne
- **Focus visible** au clavier sur les liens et boutons

![07_code_desktop_accueil](https://hackmd.io/_uploads/SJbTKOspGe.png)


![08_code_desktop_avantages](https://hackmd.io/_uploads/SyVAYujpGl.png)


![09_code_desktop_expertise_temoignages](https://hackmd.io/_uploads/BJj19dsaGe.png)


![10_code_desktop_faq](https://hackmd.io/_uploads/ryaecOj6fg.png)

Test du formulaire : après l'envoi, le message de confirmation s'affiche sur la page.

![11_code_formulaire_confirmation](https://hackmd.io/_uploads/BJbM9uiafx.png)


Sur mobile, le texte reste lisible et la page ne déborde pas sur le côté (largeur mesurée : 375 px pour un écran de 375 px).

![12_code_mobile_accueil](https://hackmd.io/_uploads/SJE7q_j6fx.png)


![13_code_mobile_avantages](https://hackmd.io/_uploads/B1vVqOjTGg.png)

---

## 4. Mesures

Mesures prises le 06/11/2026 dans le navigateur (Chrome), avec l'API `performance` du navigateur, sur la même page dans les deux versions.

| Mesure | Wix (No-Code) | HTML/CSS (Code) |
|---|---|---|
| Poids transféré | **≈ 2 006 Ko** | **≈ 11 Ko** (HTML 5,9 Ko + CSS 5,2 Ko) |
| Requêtes réseau | 203 | 2 |
| Fichiers JavaScript chargés | 93 | 0 |
| Éléments HTML dans la page | 658 | 90 |
| Premier affichage (FCP) | ≈ 440 ms | ≈ 130 ms |
| Chargement complet | ≈ 1 370 ms | ≈ 90 ms |

![14_graphique_mesures](https://hackmd.io/_uploads/B1FS9Oj6zl.png)


> Remarque honnête : ma version codée n'utilise pas de photos (dégradés CSS et icônes), alors que la version Wix charge 5 images (≈ 95 Ko). Même en ajoutant des photos optimisées, la version codée resterait autour de 150 Ko, soit plus de 10 fois plus légère. La différence vient surtout du JavaScript de Wix (≈ 1 Mo).

**Temps de réalisation** (estimation à ajuster selon ton expérience) :
- Wix : environ 2 h (choix du modèle, modification des textes, publication)
- Code : environ 4 à 5 h (structure HTML, mise en page CSS, responsive, tests)

---

## 5. Tableau comparatif

| Critère | Wix (No-Code) | HTML/CSS (Code) |
|---|---|---|
| **Coût de départ** | Gratuit, mais bandeau « Créé avec Wix » et adresse en wixsite.com | Gratuit (hébergement gratuit possible, par ex. GitHub Pages) |
| **Coût récurrent** | Forfait Premium obligatoire pour un nom de domaine et sans publicité : dès environ 17 €/mois | Nom de domaine seulement (environ 15 CHF/an) ; un développeur coûte cher s'il faut le payer |
| **Temps de réalisation** | Rapide (≈ 2 h) grâce aux modèles | Plus long (≈ 4-5 h), il faut tout écrire |
| **Compétences requises** | Aucune, glisser-déposer | HTML, CSS, notions de responsive et d'accessibilité |
| **Flexibilité / personnalisation** | Limitée au modèle et aux options de l'éditeur | Totale : chaque pixel, chaque balise est contrôlé |
| **Performance** | Page lourde : 2 Mo, 203 requêtes, 93 scripts | Page très légère : 11 Ko, 2 requêtes, 0 script |
| **Responsive mobile** | Automatique, mais résultat pas toujours bon (texte minuscule) | À faire soi-même, mais résultat maîtrisé |
| **Accessibilité** | Correcte mais images sans texte alternatif, structure de titres imposée (3 titres h1) | Structure sémantique choisie, labels, focus visible, un seul h1 |
| **Maintenance / mises à jour** | Très simple : le client modifie lui-même ses textes | Il faut savoir modifier le code (ou payer quelqu'un) |
| **Hébergement et sécurité** | Inclus et géré par Wix | À choisir soi-même (mais une page statique a très peu de risques) |
| **Dépendance** | Forte : impossible d'exporter le site en HTML, on reste chez Wix | Aucune : les fichiers peuvent être déplacés chez n'importe quel hébergeur |
| **Fonctions avancées** | Nombreuses intégrations (réservation, paiement, newsletter) en quelques clics | Il faut les coder ou ajouter des services externes |

---

## 6. Recommandation pour le client

**Client :** éléctri'tek, artisan électricien à Genève, petite équipe, pas de compétence technique en interne, besoin d'une page pour présenter une offre (borne de recharge) et recevoir des demandes de devis.

**Ma recommandation : le site codé en HTML/CSS, avec un forfait de maintenance simple.**

Arguments :
1. **Le besoin est simple et stable.** Une page de présentation avec un formulaire ne change pas toutes les semaines. Les avantages du No-Code (modifier souvent soi-même, ajouter une boutique) servent peu ici.
2. **Le coût sur la durée est plus bas.** Avec Wix, il faut payer un forfait chaque mois pour avoir son propre nom de domaine et enlever la publicité. Le site codé ne coûte que le nom de domaine chaque année.
3. **La performance compte pour un artisan.** Beaucoup de clients cherchent un électricien depuis leur téléphone. Une page de 11 Ko s'affiche instantanément, même avec une mauvaise connexion ; une page de 2 Mo est plus lente et peut être moins bien classée par Google.
4. **Image professionnelle.** Pas de bandeau « Créé avec Wix », pas de texte de modèle oublié, un rendu mobile maîtrisé.
5. **Pas de dépendance.** Le client est propriétaire de ses fichiers et peut changer d'hébergeur ou de prestataire quand il veut.

**Dans quel cas je recommanderais Wix à la place :**
- si le client veut modifier lui-même ses textes et photos très souvent ;
- s'il a besoin rapidement de fonctions avancées (prise de rendez-vous en ligne, paiement, boutique) ;
- s'il veut tester une idée en quelques heures avant d'investir (c'était l'objectif du Projet 2).

**Conclusion :** le No-Code est idéal pour aller vite et tester une idée. Pour un site durable, rapide et qui donne une image professionnelle à un artisan, le code sur-mesure est le meilleur choix, surtout quand la page est simple.

---

## 7. Livrables

- [x] Version No-Code : https://negrechesofia.wixsite.com/my-site-1
- [x] Version codée : `projet5.html` + `projet5.css`
- [x] Tableau comparatif détaillé (partie 5)
- [x] Recommandation argumentée (partie 6)