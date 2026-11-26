# Projet 4 — Refonte responsive : Boulangerie du Lac

**Classe :** E1A · **Cours :** HTML - No Code · **Élève :** Sofia Negreche

---

## 1. Analyse du site (version avant)

J'ai testé le site à 3 largeurs d'écran dans les outils de développement du navigateur (F12, puis Ctrl+Maj+M) :

- **375 px** : mobile
- **768 px** : tablette
- **1280 px** : ordinateur

## 2. Problèmes identifiés

| # | Problème | Visible sur |
|---|----------|-------------|
| P1 | Pas de balise `meta viewport`, donc le mobile affiche une page dézoomée | Mobile |
| P2 | `body` et `.hero` en `width: 1100px`, ce qui fait déborder la page avec un défilement horizontal | Mobile, tablette |
| P3 | Titre en `48px` avec `white-space: nowrap`, ce qui coupe le texte hors de l'écran | Mobile, tablette |
| P4 | Menu en `float` avec `margin-left: 40px`, ce qui fait sortir les liens de l'écran | Mobile, tablette |
| P5 | Cartes en `float` avec `width: 320px` fixe, ce qui casse la mise en page | Mobile, tablette |
| P6 | Textes trop petits (10 à 13 px), donc illisibles | Tous |

## 3. Corrections effectuées

| # | Correction | Problème réglé |
|---|-----------|----------------|
| C1 | Ajout de `<meta name="viewport" content="width=device-width, initial-scale=1.0">` | P1 |
| C2 | `box-sizing: border-box` sur tous les éléments | P2 |
| C3 | Suppression des `width` fixes | P2, P5 |
| C4 | En-tête et menu en **Flexbox** (`flex-wrap`, `gap`) | P4 |
| C5 | Titre en `rem` et suppression de `nowrap` | P3 |
| C6 | Cartes en **Grid** : 1 colonne sur mobile, 2 sur tablette, 3 sur ordinateur | P5 |
| C7 | **Media queries** `min-width: 600px` et `min-width: 1000px` (approche mobile first) | P3, P4, P5 |
| C8 | Textes à `1rem` (16 px) minimum | P6 |

### Extrait du CSS corrigé

```
css
.produits {
  display: grid;
  grid-template-columns: 1fr;
  gap: 15px;
}

@media (min-width: 600px) {
  header { flex-direction: row; justify-content: space-between; }
  .produits { grid-template-columns: repeat(2, 1fr); }
}

@media (min-width: 1000px) {
  .produits { grid-template-columns: repeat(3, 1fr); }
}
```

## 4. Captures avant / après

### Mobile (375 px)
| Avant | Après |
|-------|-------|
| ![avant-mobile-375px](https://hackmd.io/_uploads/SJadMdi6Ml.png)
 | ![apres-mobile-375px](https://hackmd.io/_uploads/SyjjMdj6zl.png)
 |

### Tablette (768 px)
| Avant | Après |
|-------|-------|
| ![avant-tablette-768px](https://hackmd.io/_uploads/SkL_mdiTzx.png)
 | ![apres-tablette-768px](https://hackmd.io/_uploads/SymPEuoTGx.png)
 |

### Ordinateur (1280 px)
| Avant | Après |
|-------|-------|
| ![avant-desktop-1280px](https://hackmd.io/_uploads/HyQ1BOo6Mg.png)
 | ![apres-desktop-1280px](https://hackmd.io/_uploads/HkayHusTGg.png)
 |

## 5. Résultat des tests

| Taille | Avant | Après |
|--------|-------|-------|
| 375 px | Débordement, texte coupé, menu invisible | 1 colonne, menu centré |
| 768 px | Débordement horizontal | 2 colonnes |
| 1280 px | Correct mais textes petits | 3 colonnes, textes lisibles |

## 6. Conclusion

Le site s'adapte maintenant à toutes les tailles d'écran grâce à la balise viewport, à Flexbox, à Grid et aux media queries.
