# Compte rendu : JavaScript 01 — Console, valeurs et variables

**Nom** : Sofia Negreche
**Date** : 28 octobre 2026
**Durée** : 45 min (estimée)
**Outils** : VS Code, Node.js v24.22.1

---

## 1. Objectif

Écrire mes premières lignes de JavaScript, les exécuter, et manipuler des variables et des types de valeurs.

## 2. Ce que j'ai fait

### Étape 1 : exécuter du JavaScript
- Création du fichier `java.js` avec `console.log("Bonjour le monde");`
- Exécution avec Node.js : `node java.js`
- Résultat : `Bonjour le monde` s'affiche.

![image](https://hackmd.io/_uploads/SJav_tJpfg.png)

> `console.log()` affiche une valeur, c'est l'outil principal pour comprendre ce que fait le code.

### Étape 2 : les variables

| Mot-clé | Usage |
| --- | --- |
| `const` | valeur qui ne sera pas réaffectée (choix par défaut) |
| `let` | valeur qui changera plus tard |
| `var` | ancienne syntaxe, à éviter |

```js
const prenom = "Sofia";
let age = 20;
age = 21;        // OK : let peut changer
// prenom = "Alex";  // erreur : Assignment to constant variable
```

![image](https://hackmd.io/_uploads/HkycdKJafe.png)


### Étape 3 : les types de valeurs

| Type | Exemple | `typeof` |
| --- | --- | --- |
| string | `"Bonjour"` | `"string"` |
| number | `42` | `"number"` |
| boolean | `true` | `"boolean"` |
| null | `null` | valeur « volontairement vide » |
| undefined | `let inconnu;` | `"undefined"` |

![image](https://hackmd.io/_uploads/ryeiut1pGl.png)


### Étape 4 : les opérateurs

| Opération | Résultat |
| --- | --- |
| `7 + 3` | 10 |
| `7 - 3` | 4 |
| `7 * 3` | 21 |
| `7 / 2` | 3.5 |
| `7 % 2` | 1 (reste de la division) |
| `2 ** 3` | 8 (puissance) |

![image](https://hackmd.io/_uploads/H182_K1pMg.png)


- Concaténation : `"Je m'appelle " + prenom + ...` ou, mieux, un **template literal** : `` `Je m'appelle ${prenom}` ``
- Comparaisons : toujours utiliser `===` et `!==` (stricts) plutôt que `==` et `!=`.
  - `5 === 5` donne `true`, `5 === "5"` donne `false` (types différents).

### Étape 5 : les pièges classiques

| Ligne | Ma prédiction | Résultat obtenu | Explication |
| --- | --- | --- | --- |
| `"5" + 5` | Je me suis trompée | `55` | le `+` concatène quand il y a du texte |
| `"5" - 2` | Je me suis trompée | `3` | le `-` convertit le texte en nombre |
| `0.1 + 0.2` | Je me suis trompée | `0.30000000000000004` | précision limitée des nombres décimaux |
| `typeof NaN` | Je me suis trompée | `number` | `NaN` est de type number |
| `Number("12") + 1` | Je me suis trompée | `13` | `Number()` convertit le texte en nombre |
| `Number("abc")` | Je me suis trompée | `NaN` | « Not a Number » : conversion impossible |

### Étape 6 : calcul du prix TTC

```js
const prixHT = 49.9;
const tva = 0.2;
const prixTTC = prixHT * (1 + tva);
console.log(`Prix HT : ${prixHT} €`);
console.log(`Prix TTC : ${prixTTC.toFixed(2)} €`);
```

Résultat : `Prix HT : 49.9 €` puis `Prix TTC : 59.88 €`. `toFixed(2)` arrondit à 2 décimales.

## 3. Défi : calcul de l'âge approximatif

```js
const anneeNaissance = 2007;
const anneeActuelle = 2026;
const ageApprox = anneeActuelle - anneeNaissance;
const moisApprox = ageApprox * 12;
console.log(`Tu as environ ${ageApprox} ans, soit environ ${moisApprox} mois.`);
```

Résultat : `Tu as environ 19 ans, soit environ 228 mois.`
Seulement des variables et des opérateurs, sans condition ni fonction. L'âge est approximatif car le mois de naissance n'est pas pris en compte.

![image](https://hackmd.io/_uploads/r100OFypzl.png)


## 4. Difficultés rencontrées et solutions

### Erreur 1 : `Identifier 'message' has already been declared`

Cause : `const message` et `const message2` étaient déclarés deux fois dans le même fichier. De plus, `sofia` était écrit sans guillemets, donc JavaScript cherchait une variable qui n'existait pas.
Solution : ne déclarer chaque variable qu'une seule fois et mettre le texte entre guillemets (ou utiliser la variable `prenom`).

![image](https://hackmd.io/_uploads/HyFWKYkpMx.png)

![image](https://hackmd.io/_uploads/B1JEYt16zl.png)


### Erreur 2 : `Cannot find module 'C:\HTML\javascript\java.js'`

Cause : le terminal était dans `C:\HTML` et le fichier ne se trouvait pas dans `javascript/`, donc Node ne le trouvait pas.
Solution : lancer `node java.js` depuis le bon dossier (ou donner le bon chemin).

![image](https://hackmd.io/_uploads/Hy7BKY1Tzg.png)


## 5. Ce que j'ai retenu

- `const` par défaut, `let` si la valeur doit changer, `var` à éviter.
- Les 5 types de base : string, number, boolean, null, undefined.
- `===` plutôt que `==` pour éviter les conversions surprises.
- Un message d'erreur indique la ligne et la raison : il faut le lire.
- Mes prédictions de l'étape 5 étaient fausses : JavaScript convertit les types automatiquement (`+` concatène, `-` calcule), et les décimaux ne sont pas parfaitement précis.

## 6. Checklist

- [x] Je sais exécuter du JavaScript dans la console et avec Node.
- [x] Je sais choisir entre `const` et `let` (et pourquoi pas `var`).
- [x] Je connais `string`, `number`, `boolean`, `null`, `undefined`.
- [x] Je sais utiliser les opérateurs, les template literals et `===`.
- [x] Je sais lire un message d'erreur.

