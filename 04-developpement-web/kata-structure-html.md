# Structure HTML — Kata A et Kata B

**Nom :** Negreche Sofia — Classe E1A
**Cours :** Structure HTML — Kata

[TOC]

## 1. Objectif

Répéter, jusqu'à la fluidité, la construction d'un squelette HTML correct et bien indenté, sans regarder de référence, en 5 répétitions minimum par kata.

Katas choisis :
- **Kata A — Page minimale valide**
- **Kata B — Section hiérarchisée**

## 2. Procédure (commune aux deux katas)

1. Lancer le chronomètre.
2. Créer un nouveau fichier dans VS Code (`struc-1.html`, `struc-2.html`, …).
3. Écrire le code de mémoire, sans modèle.
4. Indenter de 4 espaces à chaque niveau d'imbrication.
5. Copier le code dans le validateur W3C (https://validator.w3.org/nu, « text input »).
6. Arrêter le chronomètre quand le validateur affiche le résultat.
7. Ouvrir le fichier dans le navigateur pour vérifier le rendu.
8. Noter le temps, les erreurs et ce qui a changé depuis l'essai précédent.

**Outils :** VS Code, navigateur, validateur W3C Nu HTML Checker, chronomètre.

---

## 3. Kata A — Page minimale valide

### 3.1 Consigne

Écrire de mémoire un document complet avec :
- `<!DOCTYPE html>`
- `<html lang="fr">`
- un `<head>` avec `<meta charset="UTF-8">` et `<title>`
- un `<body>` vide

### 3.2 Essai 1 — `struc-1.html`

- **Temps :** 1 min 47 s
- **Erreurs au validateur :** 2
  - « Saw an end tag after body had been closed »
  - « Stray end tag head »
- **Justesse :**
  - `<body>` placé à l'intérieur du `<head>` (cause des 2 erreurs) ;
  - `<title>` écrit avant `<meta charset>` (l'ordre conseillé est `meta charset` en premier) ;
  - indentation correcte.
- **Ce qui a changé :** premier essai, point de départ.

![image](https://hackmd.io/_uploads/r1ffaEAnMx.png)
![image](https://hackmd.io/_uploads/ryzm6V0nzg.png)

### 3.3 Essai 2 — `struc-2.html`

- **Temps :** 1 min 37 s (10 s de moins)
- **Erreurs au validateur :** 0
- **Justesse :**
  - `meta charset` remis avant `title` ;
  - plus de `<body>` dans le `<head>`… mais le `<body>` a été oublié ;
  - espace avant le texte du titre (`<title> kata-a`) ;
  - au premier contrôle, le code a été collé par erreur dans le validateur **CSS**, qui a affiché 2 erreurs car il ne lit pas le HTML.
- **Ce qui a changé :** les 2 erreurs de l'essai 1 ont disparu, l'ordre dans le `<head>` est correct.

![image](https://hackmd.io/_uploads/HJQEaNA2Me.png)
![image](https://hackmd.io/_uploads/SkSsaN03Ml.png)
![image](https://hackmd.io/_uploads/rkwn6NCnzl.png)


### 3.4 Essai 3 — `struc-3.html`

- **Temps :** 1 min 00 s (37 s de moins)
- **Erreurs au validateur :** 0
- **Justesse :**
  - `<body>` toujours absent ;
  - faute de frappe dans le titre : `kata-8` au lieu de `kata-a`, avec des espaces autour ;
  - `<head>` et `</head>` pas indentés sous `<html>` ;
  - ligne vide inutile avant `</head>`.
- **Ce qui a changé :** gros gain de temps (−37 s), structure écrite plus vite, mais quelques détails de soin en moins.

![image](https://hackmd.io/_uploads/HylRa6NAhMe.png)
![image](https://hackmd.io/_uploads/ryKRaV03Ge.png)


### 3.5 Essai 4 — `struc-4.html`

- **Temps :** 47 s (13 s de moins)
- **Erreurs au validateur :** …
- **Justesse :**
  - titre corrigé (`kata-a`) ;
  - `<head>` de nouveau bien indenté ;
  - `<body>` toujours absent ;
  - reste une ligne vide avant `</head>` et un espace avant le texte du titre.
- **Ce qui a changé :** meilleur temps, indentation retrouvée, faute du titre corrigée.

![image](https://hackmd.io/_uploads/By21A4Ahzg.png)
![image](https://hackmd.io/_uploads/HkYgCV0hGl.png)


### 3.6 Essai 5 — `struc-5.html`

- **Temps :** …
- **Erreurs au validateur :** …
- **Justesse :** …
- **Ce qui a changé :** …

Capture : code — …
Capture : validateur — …

### 3.7 Récapitulatif Kata A

| Essai | Fichier | Temps | Écart | Erreurs | Ce qui a changé |
|:-----:|---------|:-----:|:-----:|:-------:|-----------------|
| 1 | struc-1.html | 1:47 | — | 2 | Point de départ, `<body>` dans le `<head>` |
| 2 | struc-2.html | 1:37 | −10 s | 0 | Ordre du `<head>` corrigé, `<body>` oublié |
| 3 | struc-3.html | 1:00 | −37 s | 0 | Plus rapide, faute `kata-8`, indentation du `<head>` |
| 4 | struc-4.html | 0:47 | −13 s | … | Titre et indentation corrigés |
| 5 | struc-5.html | … | … | … | … |

### 3.8 Bilan Kata A

- **Temps :** de 1:47 (essai 1) à 0:47 (essai 4), soit **1 minute de moins** : plus de deux fois plus rapide.
- **Erreurs au validateur :** de 2 (essai 1) à 0 dès l'essai 2.
- **Point à retenir :** le validateur ne signale pas l'absence du `<body>`, mais la consigne l'exige. Il faut donc relire son code en plus de le valider.
- **Ce que j'ai retenu :**
  - le `<body>` est obligatoire dans le kata, même vide, et il vient **après** `</head>` ;
  - `meta charset` se place en premier dans le `<head>` ;
  - le texte du `<title>` s'écrit sans espace autour ;
  - le validateur HTML est validator.w3.org/nu, pas le validateur CSS.

---

## 4. Kata B — Section hiérarchisée

### 4.1 Consigne

Reproduire de mémoire l'arborescence imposée, avec une indentation parfaite :

```
article
├── h1
├── section
│   ├── h2
│   ├── p
│   └── p
└── section
    ├── h2
    ├── p
    └── p
```

### 4.2 Essai 1 — `struct-1.html`

- **Temps :** …
- **Erreurs au validateur :** 0, mais la structure était fausse.
- **Justesse :**
  - `<title>` pas fermé : tout le reste de la page s'est retrouvé à l'intérieur ;
  - `<body>` placé dans le `<head>` ;
  - `<article>` fermé juste après le `<h1>`, les sections étaient en dehors ;
  - `<p>` placés à l'intérieur des `<h2>` ;
  - nombre de `<p>` ouverts et fermés différent dans les deux sections.
- **Pourquoi le validateur n'a rien vu :** le navigateur lit tout ce qui suit `<title>` comme du simple texte de titre, jusqu'au `</title>`. Les balises mal placées n'étaient donc pas analysées comme du HTML.
- **Ce qui a changé :** premier essai, point de départ.

Capture : validateur — Capture_d_écran_2026-10-27_115217.png

### 4.3 Essai 2 — `struct-2.html`

- **Temps :** …
- **Erreurs au validateur :** …
- **Justesse :**
  - structure complète et correcte : `<head>` fermé avant `<body>`, deux `<section>` dans l'`<article>`, `<h2>` suivi de deux `<p>` ;
  - indentation correcte à tous les niveaux ;
  - détails : espaces autour de certains textes, majuscules irrégulières (`Paragraphe 1` / `paragraphe 2`), faute « Titre principale ».
- **Ce qui a changé :** toutes les erreurs de structure de l'essai 1 sont corrigées.
![image](https://hackmd.io/_uploads/SJXBR4R2zl.png)


Capture : validateur — …

### 4.4 Essai 3 — `struct-3.html`

- **Temps :** …
- **Erreurs au validateur :** …
- **Justesse :** …
- **Ce qui a changé :** …

Capture : code — …
Capture : validateur — …

### 4.5 Essai 4 — `struct-4.html`

- **Temps :** …
- **Erreurs au validateur :** …
- **Justesse :** …
- **Ce qui a changé :** …

Capture : code — …
Capture : validateur — …

### 4.6 Essai 5 — `struct-5.html`

- **Temps :** …
- **Erreurs au validateur :** …
- **Justesse :** …
- **Ce qui a changé :** …

Capture : code — …
Capture : validateur — …

### 4.7 Récapitulatif Kata B

| Essai | Fichier | Temps | Erreurs | Ce qui a changé |
|:-----:|---------|:-----:|:-------:|-----------------|
| 1 | struct-1.html | … | 0 (structure fausse) | Point de départ |
| 2 | struct-2.html | … | … | Structure entièrement corrigée |
| 3 | struct-3.html | … | … | … |
| 4 | struct-4.html | … | … | … |
| 5 | struct-5.html | … | … | … |

### 4.8 Bilan Kata B

- **Temps :** de … (essai 1) à … (essai 5).
- **Justesse :** structure fausse à l'essai 1, correcte dès l'essai 2.
- **Ce que j'ai retenu :**
  - le `<head>` se ferme avant d'ouvrir le `<body>` ;
  - le `<title>` se ferme tout de suite, sinon tout le reste devient du texte ;
  - un `<h2>` ne contient que du texte : les `<p>` viennent après lui, au même niveau ;
  - les deux `<section>` sont à l'intérieur de l'`<article>` ;
  - « 0 erreur » au validateur ne veut pas dire que la structure est bonne : il faut aussi relire.

---

## 5. Conclusion

En répétant les katas, j'écris la structure HTML plus vite et plus juste. Sur le Kata A, je suis passée de 1:47 à 0:47 et de 2 erreurs à 0. Sur le Kata B, la structure est devenue correcte dès le 2e essai. J'ai aussi appris que le validateur ne suffit pas : il ne voit pas un `<body>` oublié ni une structure cachée dans un `<title>` mal fermé, il faut donc toujours relire son code.
