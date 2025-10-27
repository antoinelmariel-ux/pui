# Swipe Quiz

## Aperçu
Ce projet est une application web autonome qui propose un quiz « vrai ou faux » avec une interface de cartes à faire glisser. Toutes les interactions sont gérées par du JavaScript vanilla dans un seul fichier HTML.

## Structure du fichier HTML
- **`<head>`** : définit les métadonnées et contient l'intégralité des styles CSS pour la mise en page (dégradé de fond, cartes, animations, boutons circulaires, etc.).
- **`<body>`** : se limite à un élément racine `<div id="app"></div>` rempli dynamiquement par la classe `SwipeQuizGame`.

### Mise en page principale
- `.container` centre verticalement le contenu sur l'écran et sert de cadre aux deux écrans (jeu et fin).
- `.header` affiche le titre « Quiz Swipe » et deux pastilles de statistiques : index de la question et score en cours.
- `.card-area` empile la carte active (`.main-card`), la carte suivante (`.next-card`) et la superposition de feedback (`.feedback-overlay`). Les indicateurs latéraux `.indicator.left` et `.indicator.right` deviennent visibles pendant le glisser pour suggérer « Faux » ou « Vrai ».
- `.instructions` propose un rappel visuel du geste attendu (flèches gauche/droite) et `.action-buttons` fournit deux boutons circulaires pour répondre sans swipe.
- `.help-text` affiche une assistance textuelle et le `footer` ajoute la version de l'application (`Version 1.1.0`).

### Animations et effets
- Les animations CSS `bounce`, `spin`, `pulse`, `ping` et `fadeInUp` animent respectivement le feedback, l'icône de validation, l'intensité du retour et l'apparition de l'écran final.
- `createParticles()` génère huit confettis `.particle` lorsqu'une réponse est correcte pour renforcer le feedback positif.

### Écran de fin
- `.end-screen` affiche un trophée SVG, le score final et la liste détaillée des réponses. Un bouton « Rejouer » réinitialise l'état du quiz. Le `footer` est réutilisé dans une variante claire (`class="footer light"`) adaptée au fond blanc de cet écran.

## Logique JavaScript
- La classe `SwipeQuizGame` encapsule l'état du jeu : index de carte (`currentCard`), score, état de drag et historique des réponses.
- `loadQuestions()` récupère désormais le contenu des cartes depuis le fichier `cards.json`, puis alimente `this.questions`.
- `this.questions` contient les cartes chargées, chacune sous la forme `{ question, answer, explanation }`, parcourues séquentiellement.
- `handleStart`, `handleMove` et `handleEnd` gèrent le glisser-déposer : calcul de l'offset, application des transformations CSS (`updateCardStyle`) et déclenchement de `animateCardExit` si le seuil horizontal est dépassé.
- `animateCardExit` choisit la direction de sortie de la carte, prépare l'affichage du feedback (via `showFeedback`) puis délègue à `handleAnswer` pour calculer le score et avancer dans la pile de questions.
- `handleAnswer` stocke le résultat, incrémente le score si nécessaire et appelle `render()` pour mettre à jour l'interface. Lorsque toutes les cartes sont jouées, `gameEnded` passe à `true` pour afficher l'écran de fin.
- `renderGameScreen()` et `renderEndScreen()` retournent l'HTML des deux écrans. `bindGameEvents()` connecte les boutons `Vrai/Faux` et les interactions tactiles à la logique de drag.
- Des écrans dédiés (`renderLoadingScreen`, `renderErrorScreen`, `renderEmptyState`) affichent l'état du chargement des cartes et proposent un bouton pour relancer la récupération en cas de problème.
- `resetGame()` réinitialise l'état pour relancer une partie après le quiz.

## Personnalisation
- Ajoutez ou modifiez les questions directement dans le fichier `cards.json` pour adapter le quiz à un nouveau sujet.
- Ajustez les styles CSS intégrés pour changer la palette de couleurs, les animations ou la disposition.
- Intégrez le quiz dans un site existant en copiant le fichier `Swipe quiz.html` ou en extrayant la classe `SwipeQuizGame` dans un module JavaScript.

## Fichier `cards.json`
- Structure : un objet racine possédant une clé `cards` contenant un tableau d'objets `{ "question": string, "answer": boolean, "explanation": string }`.
- Le fichier est formaté pour rester lisible et facilement éditable à la main.
- Lors du rechargement du quiz, le bouton « Réessayer » permet de relire `cards.json` sans rafraîchir la page.

## Utilisation
Servez le dossier via un petit serveur web local (ex. `npx serve .` ou `python -m http.server`) puis ouvrez `Swipe quiz.html` dans un navigateur moderne ; aucune dépendance externe n'est nécessaire.

## Backoffice de gestion
- Le fichier `backoffice.html` fournit une interface complète pour gérer les cartes : création, modification et suppression.
- Vous pouvez importer un fichier `cards.json` existant, le modifier et l'exporter à nouveau en un clic.
- Le bouton « Charger depuis cards.json » relit directement le fichier du dépôt pour partir de la base actuelle.

## Version
Footer mis à jour : **Version 1.3.0** avec la mention « Le contenu de ce module est entièrement sous la responsabilité de l’établissement ».
