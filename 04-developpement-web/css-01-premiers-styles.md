# CSS 01 — Mes premiers styles

**Date :** 29 octobre 2026
**Outils :** VS Code, Firefox
**Durée :** environ 40 minutes

## Objectif

Relier une feuille de style à une page HTML, cibler des éléments avec des sélecteurs de base, et changer couleurs, polices et fonds.

> Le HTML décrit le contenu, le CSS décrit l'apparence.
> Une règle CSS = un **sélecteur** (quoi cibler) + des **déclarations** (quel style appliquer).

```css
p {
  color: navy;
}
```

---

## 1. Préparer l'environnement dans VS Code

- Ouverture du dossier **Cours-code** dans VS Code (Fichier → Ouvrir le dossier).
- Création d'un sous-dossier `html`, puis des fichiers `presentation.html` et `style.css`.

![image](https://hackmd.io/_uploads/rkDXXMZaMe.png)


![image](https://hackmd.io/_uploads/H1LHmzZafl.png)


**Erreurs rencontrées et corrigées :**
- Le fichier s'appelait `stylue.css` au lieu de `style.css` : le HTML ne le trouvait pas. Renommé avec `F2`.
- Le code était collé deux fois dans les deux fichiers : doublons supprimés.

![image](https://hackmd.io/_uploads/rkhw7zbpze.png)

---

## 2. Ouvrir la page dans le navigateur

- Clic droit sur `presentation.html` → **Reveal in File Explorer** → double-clic.
- Après chaque modification : `Ctrl+S` dans VS Code, puis `F5` dans le navigateur.

![image](https://hackmd.io/_uploads/HyvF7GWazg.png)


**À retenir :** `F5` dans VS Code lance le débogage et crée un fichier `launch.json` inutile. Le `F5` utile est celui du navigateur.

![image](https://hackmd.io/_uploads/BJo97MWTMg.png)


---

## 3. Étape 1 : relier la feuille de style

Dans le `<head>` de `presentation.html` :

```html
<link rel="stylesheet" href="style.css">
```

Dans `style.css` :

```css
h1 {
  color: tomato;
}
```

Le titre devient rouge-orangé.

---

## 4. Étape 2 : cibler par balise et par classe

```css
p {
  color: #333333;
}

.important {
  color: crimson;
  font-weight: bold;
}
```

Dans le HTML : `<p class="important">…</p>`

Tous les paragraphes sont gris foncé, sauf le paragraphe « important » en rouge et en gras.

![image](https://hackmd.io/_uploads/SkDn7MZafe.png)


| Sélecteur | Écriture | Cible |
|---|---|---|
| Balise | `p { … }` | toutes les balises `<p>` |
| Classe | `.important { … }` | tout élément avec `class="important"` |
| Identifiant | `#titre-principal { … }` | l'élément avec cet `id` (unique) |

---

## 5. Étape 3 : les couleurs

```css
body {
  background-color: #f4f4f4;
  color: #222222;
}

header {
  background-color: #1e90ff;
  color: white;
}
```

Fond de page gris clair et bande bleue autour du titre.
Le titre reste rouge-orangé, car `h1 { color: tomato; }` cible directement le titre. Rouge sur bleu se lit mal.

![image](https://hackmd.io/_uploads/BJn6XfbTfg.png)


Quatre façons d'écrire une couleur : `red`, `#1e90ff`, `rgb(30, 144, 255)`, `hsl(210, 100%, 56%)`.

---

## 6. Étape 4 : le texte

```css
body {
  font-family: Arial, Helvetica, sans-serif;
  font-size: 18px;
  line-height: 1.6;
}

h1 {
  text-align: center;
  text-transform: uppercase;
}

a {
  text-decoration: none;
}
```

Police Arial, titre en majuscules et centré, lien sans soulignement.

![image](https://hackmd.io/_uploads/ByW1VzbpMx.png)

---

## 7. Étape 5 : regrouper et survoler

```css
h1, h2, h3 {
  font-family: Georgia, serif;
}

a:hover {
  color: crimson;
  text-decoration: underline;
}
```

Les titres passent en Georgia, et le lien devient rouge et souligné au survol de la souris.

![image](https://hackmd.io/_uploads/r1BgVM-pGe.png)


![image](https://hackmd.io/_uploads/r1I-4zZpzx.png)

---

## 8. Étape 6 : inspecter avec les DevTools (F12)

- Onglet **Inspecteur** → clic sur un `<p>` → panneau **Styles**.
- On peut décocher une propriété ou changer une valeur en direct.
- `F5` remet la page à l'état du fichier : rien n'est modifié dans `style.css`.

![image](https://hackmd.io/_uploads/SJhzEzb6zg.png)


**Erreur rencontrée :** j'ai tapé `#blue` au lieu de `blue`. Le `#` ne sert que pour les codes hexadécimaux. Le navigateur a barré la règle avec un avertissement.

![image](https://hackmd.io/_uploads/SkhQNGb6Mx.png)


![image](https://hackmd.io/_uploads/HJA4NfbTGe.png)

---

## 9. Le défi : `.carte` et `.carte-titre`

Dans le HTML, uniquement des attributs `class` :

```html
<section class="carte">
  <h2 class="carte-titre">Ma première carte</h2>
  <p>Un texte dans la carte.</p>
</section>
```

Dans `style.css` :

```css
.carte {
  background-color: #eef6ff;
  color: #1a2b3c;
  font-family: Georgia, serif;
}

.carte-titre {
  color: #1e90ff;
  font-family: Arial, Helvetica, sans-serif;
  text-transform: uppercase;
}
```

Carte avec un fond bleu très clair, titre en bleu, majuscules, Arial.

![image](https://hackmd.io/_uploads/HJRrEfb6ze.png)

---

## 10. Test du sélecteur par id

```html
<h1 id="titre-principal">Bienvenue sur ma page</h1>
```

```css
#titre-principal {
  color: white;
}
```

![image](https://hackmd.io/_uploads/BJtvEM-TMe.png)

**Problème rencontré :** le titre et la bande bleue ont disparu. Le texte du `<h1>` était une suggestion grisée de l’éditeur, donc le titre était vide et sans hauteur.
![image](https://hackmd.io/_uploads/BJkGBM-6ze.png)

![image](https://hackmd.io/_uploads/rJrxSzW6fl.png)


![image](https://hackmd.io/_uploads/H1WsNGZpfl.png)

**Solution :** retaper le texte à la main, sans la suggestion grise.

![image](https://hackmd.io/_uploads/ryQ2EMWTzg.png)

Le titre est blanc sur la bande bleue : l'`id` l'emporte sur la règle `h1`.

![image](https://hackmd.io/_uploads/HkXa4zbazx.png)


---

## Checklist

- [x] `style.css` est reliée par `<link>` et fonctionne
- [x] Je sais écrire une règle (sélecteur, propriété, valeur)
- [x] Je sais cibler par balise, par classe et par id
- [x] Je sais changer couleur de texte, fond, police, alignement
- [x] Je sais utiliser `:hover` et inspecter un style dans les DevTools

## Ce que j'ai retenu

- Toujours **sauvegarder** (`Ctrl+S`) avant de rafraîchir (`F5`).
- Un **point-virgule** ou une **accolade** oubliés cassent la règle.
- Un **id** l'emporte sur une **classe**, qui l'emporte sur une **balise** : c'est le sujet de CSS 02.
- Préférer les **classes** pour styler, l'id est unique par page.
- Se méfier des **suggestions grisées** de l’éditeur : ce n'est pas du vrai code.

