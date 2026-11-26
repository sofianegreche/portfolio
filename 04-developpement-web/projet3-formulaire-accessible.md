# Projet 3 – Formulaire de contact accessible

**Auteur :** Sofia Negreche
**Classe :** E1A
**Cours :** HTML - No Code
**Durée :** 1 semaine (PPE)
**Fichiers :** `projet3.html`, `projet3.css`

---

## 1. Contexte

J'ai réalisé un formulaire de contact HTML accessible pour l'entreprise d'électricité **éléctri'tek**. J'ai adapté le contexte du sujet (une association) à mon entreprise : les critères d'accessibilité appliqués restent les mêmes.

Le formulaire contient :
- les coordonnées : nom, e-mail, téléphone (facultatif) ;
- la demande : type d'intervention (dépannage, installation, mise aux normes, demande de devis, autre) et message.

---

## 2. Critères d'accessibilité appliqués

| Critère | Mise en œuvre | Pourquoi |
|---|---|---|
| Langue de la page | `<html lang="fr">` | Le lecteur d'écran prononce correctement le français |
| Structure sémantique | `header`, `nav`, `main`, `section`, `form`, `footer`, un seul `h1` | Repères de navigation pour les technologies d'assistance |
| Labels | `<label for>` relié à chaque `id` | Chaque champ est annoncé clairement, et un clic sur le label place le curseur dans le champ |
| Regroupement | `<fieldset>` + `<legend>` (« Vos coordonnées », « Votre demande ») | Les champs liés sont annoncés ensemble |
| Types adaptés | `type="email"`, `type="tel"`, `<select>`, `<textarea>` | Clavier adapté sur mobile et validation native |
| Saisie facilitée | `autocomplete` (name, email, tel) | Moins de saisie pour l'utilisateur |
| Champs obligatoires | `required` + étoile (*) + phrase d'explication | L'obligation n'est pas indiquée par la couleur seule |
| Validation HTML5 | `required`, `type="email"`, `pattern` (téléphone), `minlength` (message) | Erreurs détectées côté client, avec des messages natifs du navigateur |
| Aide à la saisie | `aria-describedby` + `title` sur le téléphone | Le format attendu est annoncé |
| Navigation clavier | Ordre logique du HTML + lien d'évitement « Aller au contenu » | Utilisable sans souris |
| Focus visible | Contour bleu de 3 px (`:focus`) | On voit toujours où l'on se trouve |
| Erreurs visibles | Bordure rouge épaisse (`:user-invalid`) + message natif en texte | L'erreur ne dépend pas que de la couleur |
| Confirmation | `role="status"` + `focus()` sur le message | Annoncé par les lecteurs d'écran, le focus ne se perd pas |
| Zones tactiles | Champs et bouton d'au moins 44 px de haut | Facile à cliquer |
| Unités relatives | `rem` et largeur maximale de 40 rem | Texte agrandissable, lecture confortable |

---

## 3. Contrastes (ratio WCAG, minimum demandé : 4,5:1)

| Éléments | Couleurs | Ratio |
|---|---|---|
| Texte principal / fond | #1a1a1a sur #ffffff | 17,4:1 |
| Liens / fond | #0b4fa8 sur #ffffff | 7,8:1 |
| Texte du bouton / fond du bouton | #ffffff sur #0b4fa8 | 7,8:1 |
| Bouton au survol | #ffffff sur #083a7c | 11,0:1 |
| Texte d'aide (format) | #444444 sur #ffffff | 9,7:1 |
| Contour de focus | #005fcc sur #ffffff | 6,0:1 |
| Bordure d'erreur | #b00020 sur #ffffff | 7,3:1 |
| Message de confirmation | #1a1a1a sur #e8f5e9 | 15,5:1 |

Tous les ratios dépassent le minimum de 4,5:1.

---

## 4. Preuve de test au clavier (sans souris)

Test réalisé dans le navigateur, uniquement avec les touches `Tab`, les flèches, `Alt + ↓` et `Entrée`.

### Capture 1 : lien d'évitement

Au premier appui sur `Tab`, le lien « Aller au contenu » apparaît en haut à gauche, entouré d'un contour bleu.

![image](https://hackmd.io/_uploads/S1mQCvi6fl.png)


### Capture 2 : focus sur un champ

Avec `Tab`, le focus arrive sur « Votre nom ». Le contour bleu et le curseur sont bien visibles.

![image](https://hackmd.io/_uploads/S144AvoaGg.png)

### Capture 3 : liste déroulante utilisable au clavier

Avec `Tab` jusqu'à « Type d'intervention », puis `Alt + ↓`, la liste s'ouvre et les flèches permettent de choisir une option.

![image](https://hackmd.io/_uploads/SyIBCvsaze.png)


### Capture 4 : focus sur le bouton

Avec `Tab` jusqu'à « Envoyer », le bouton est entouré d'un contour bleu.

![image](https://hackmd.io/_uploads/SyPI0Po6fg.png)


### Capture 5 : message d'erreur

Sur le formulaire vide, `Entrée` bloque l'envoi. Le navigateur place le focus sur le premier champ manquant et affiche « Veuillez compléter ce champ. ». La bordure du champ devient rouge et plus épaisse : l'erreur ne dépend donc pas que de la couleur.

![image](https://hackmd.io/_uploads/SkaP0vi6Ge.png)


### Capture 6 : confirmation après un envoi valide

Avec le formulaire correctement rempli, `Entrée` affiche le message « Merci, votre message a bien été envoyé. » sur la page (annoncé par `role="status"`, avec le focus).

![image](https://hackmd.io/_uploads/B1lKCvspGl.png)


**Résultat :** tous les champs et le bouton sont atteignables dans un ordre logique, le focus est toujours visible, les erreurs sont signalées et le formulaire s'envoie avec la touche `Entrée`.

---

## 5. Limites

- Le formulaire n'envoie pas réellement le message : sans serveur, l'envoi est simulé côté navigateur (message de confirmation en JavaScript). Un vrai envoi demanderait un service ou du code côté serveur.
- Le test a été fait avec le clavier et les outils du navigateur, pas avec un vrai lecteur d'écran (NVDA, VoiceOver).
- La pseudo-classe `:user-invalid` n'est pas reconnue par les très anciens navigateurs ; la validation native reste active dans tous les cas.

---

## 6. Conclusion

Le formulaire respecte les bonnes pratiques d'accessibilité de base : HTML sémantique, labels et regroupements, contrastes suffisants, focus visible, validation HTML5 native et navigation complète au clavier.