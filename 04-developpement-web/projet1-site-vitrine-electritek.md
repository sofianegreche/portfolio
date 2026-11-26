# Projet 1 – Site vitrine éléctri'tek

**Classe :** E1A | **Niveau :** Débutant | **Cours de référence :** HTML - No Code

## 1. Contexte et objectif

Création d'un site vitrine pour un artisan électricien. Le site présente ses services, permet aux clients de demander un devis, de suivre leur demande, et donne au professionnel un espace pour recevoir et traiter les demandes.

## 2. Structure du projet

```
projets.html/
├── index.html      → page d'entrée avec les cases de navigation
├── accueil.html    → présentation de l'artisan
├── services.html   → services cliquables + formulaire de devis
├── contact.html    → formulaire de demande d'intervention
├── suivi.html      → suivi de la demande par le client
├── admin.html      → espace professionnel (connexion + demandes)
└── projet1.css     → feuille de style commune
```

**Choix de structure :** une page par thème pour que chaque page ait un contenu clair. Chaque page reprend la même structure sémantique : `header`, `nav`, `main`, `section`, `article`, `footer`.

## 3. Étapes réalisées

### 3.1 Base HTML et CSS
- Mise en place de la structure sémantique et du lien vers `projet1.css`.
- Mise en forme : couleurs (bleu foncé et jaune), polices, cartes blanches avec coins arrondis, mise en page en `flex`.
- Responsive de base avec une `@media (max-width: 600px)`.

### 3.2 Corrections d'erreurs rencontrées

| Erreur | Correction |
|---|---|
| `id="titre principale"` (espace) | `id="titre-principale"` |
| Balise `<p` non fermée | balise complétée et fermée |
| `/article>` sans `<` (un `<` s'affichait sur la page) | `</article>` |
| `<p class="important"...></p` mal écrit | `<p class="important">...</p>` |
| Liens `#Services` alors que `id="services"` | ancres en minuscules (sensibles à la casse) |
| `href="style.css"` alors que le fichier est `projet1.css` | `href="projet1.css"` |
| Code JavaScript collé sans balise | ajout de `<script>...</script>` avant `</body>` |

### 3.3 Passage à un site à plusieurs pages
- `index.html` : cases Accueil, Services, Contact, Suivi qui ouvrent chacune une page différente.
- Menu de navigation identique sur toutes les pages.
- Lien « Espace professionnel » dans le pied de page.

### 3.4 Page Services interactive
- Les trois services (Dépannage, Installation, Mise aux normes) sont des cartes **cliquables**.
- Au clic, la carte est mise en surbrillance et un formulaire de devis apparaît avec le service déjà choisi.
- Champs : nom, prénom, e-mail, téléphone, adresse, message.

### 3.5 Page Contact
- Formulaire avec nom, prénom, e-mail, téléphone, type d'intervention (liste déroulante) et message.

### 3.6 Numéro de suivi et page Suivi
- Chaque demande reçoit un numéro de suivi unique du type `ET-123456`.
- Le client saisit son numéro et son e-mail pour voir le statut de sa demande et la réponse du professionnel.

### 3.7 Espace administrateur
- Connexion par identifiant et mot de passe (identifiants de démonstration : `admin` / `admin`).
- Affichage de toutes les demandes reçues (devis et contact) avec les coordonnées du client.
- Changement du statut : Nouvelle, En cours, Répondu, Terminée.
- Zone de réponse préremplie et bouton « Répondre par e-mail » qui ouvre la messagerie avec le message prêt et passe la demande en « Répondu ».
- Bouton pour supprimer une demande et bouton de déconnexion.

## 4. Notions JavaScript utilisées

| Notion | Utilisation |
|---|---|
| `document.getElementById` | récupérer un élément de la page |
| `addEventListener` | réagir à un clic ou à l'envoi d'un formulaire |
| `e.preventDefault()` | empêcher le rechargement de la page à l'envoi |
| `localStorage` | enregistrer les demandes dans le navigateur |
| `sessionStorage` | garder la connexion admin le temps de la session |
| `JSON.parse` / `JSON.stringify` | convertir les données en texte et inversement |
| `createElement` et `textContent` | construire les cartes de demandes de façon sûre |
| `forEach`, `find`, `splice` | parcourir, chercher et supprimer des demandes |
| attribut `hidden` | afficher ou cacher une section |

## 5. Fonctionnement global

1. Le client choisit un service ou remplit le formulaire de contact.
2. La demande est enregistrée avec un numéro de suivi.
3. Le professionnel se connecte sur `admin.html`, lit la demande, change son statut et répond.
4. Le client consulte `suivi.html` pour voir le statut et la réponse.

## 6. Limites du projet

- **Sécurité factice :** l'identifiant et le mot de passe sont écrits dans le code, donc visibles par n'importe qui. Un vrai site demanderait un serveur et une base de données.
- **Stockage local :** les demandes restent dans le navigateur de l'ordinateur utilisé. Le suivi ne marche que si le client et l'admin utilisent le même navigateur.
- **E-mail :** `mailto:` ouvre simplement la messagerie de l'utilisateur, il n'envoie rien automatiquement.

## 7. Reste à faire

- [ ] Tester le site dans au moins 2 navigateurs
- [ ] Faire une capture d'écran desktop et une capture mobile
- [ ] Rédiger la note expliquant les choix de structure
- [ ] Remplacer le texte de présentation dans `accueil.html`