# Prompt de design — Jeu "Unités, multiples et sous-multiples"

À utiliser dans Claude Design pour produire et valider les maquettes avant le passage en développement sous Claude Code.

---

## Contexte du projet

Jeu web pédagogique destiné à des lycéens de STI2D (filière technologique française), pour s'entraîner aux conversions d'unités avec préfixes (giga à nano), en incluant la notion de puissance de 10. Reprend l'esprit du jeu "Formules en jeu" déjà réalisé : design moderne, attrayant, interactif, avec glisser-déposer.

## Structure à maquetter

### Écran 1 — Accueil / sélection de bloc

- Titre de l'application en couleur unie (pas de dégradé de texte).
- 4 cartes représentant les 4 blocs de difficulté, affichées en grille :
  1. Unités simples — écarts faibles
  2. Unités simples — écarts grands
  3. Surfaces et volumes
  4. Grandeurs composées
- Chaque carte affiche : titre du bloc, icône ou pictogramme représentatif, état (verrouillé / débloqué / déjà joué), et si déjà joué : meilleur score et étoiles obtenues.
- Les blocs verrouillés sont visuellement distincts (grisés, cadenas) mais restent lisibles — pas de mystère total sur le contenu à venir.
- Commutateur thème clair/sombre dans l'en-tête, cohérent avec les autres outils déjà réalisés.

### Écran 2 — Écran de jeu (question en cours)

- Compteur de progression "Question X / 20" bien visible.
- Zone centrale de la question, dont l'apparence change selon le format d'exercice tiré :
  - **Format direct** : la valeur de départ affichée en grand, un champ de saisie, éventuellement un sélecteur d'unité cible.
  - **Format QCM** : l'énoncé de la conversion, 4 boutons de proposition (× 10³ / ÷ 10³ / × 10⁻³ / ÷ 10⁻³ ou équivalent).
  - **Format glisser-déposer** : zone(s) de dépôt clairement délimitées, éléments déplaçables (cartes-valeurs ou cartes-puissances de 10) dans une palette en bas ou sur le côté.
- Feedback visuel net après validation : succès ou erreur, avec des couleurs vérifiées en simulation daltonienne (les variables `--success-*` et `--danger-*` du système déjà en place restent utilisables ici, distinctes de la paire Okabe-Ito réservée aux comparaisons d'éléments).
- Score cumulé de la série visible en permanence, discret mais présent.

### Écran 3 — Résultats de série

- Score final sur 20, étoiles (barème à afficher clairement, ex. seuils visibles).
- Message d'encouragement adapté au score.
- Tableau récapitulatif : une ligne par question (énoncé résumé / réponse donnée / réponse correcte / ✓ ou ✗).
- Boutons d'action : "Recommencer ce bloc", "Retour à l'accueil", et "Bloc suivant" si applicable (visible seulement si le bloc suivant vient d'être débloqué par ce score).

## Direction visuelle

- **Palette Okabe-Ito** comme palette d'accent unique par défaut : bleu `#0072B2`, orange `#E69F00`. Utilisée pour distinguer les éléments à comparer visuellement (par exemple les deux facteurs d'une grandeur composée, ou les deux issues possibles d'un QCM).
- **Police Verdana** partout.
- **Thème clair/sombre** commutable, avec un jeu de variables CSS cohérent (fonds à 3 niveaux de profondeur, textes hiérarchisés, bordures discrètes/marquées).
- Style moderne et attrayant, dans l'esprit d'une application d'apprentissage ludique actuelle plutôt qu'un support de cours classique — cartes avec ombre douce, coins arrondis, micro-animations de transition entre les états (apparition des feedbacks, changement d'écran).
- Un accent doré réservé exclusivement à la mise en évidence d'un point clé (cohérent avec la convention déjà en place sur les autres outils : le doré signale toujours l'élément à repérer en priorité, jamais un usage décoratif générique).
- Titre toujours en couleur unie, jamais de dégradé de texte.

## Éléments d'interaction à illustrer

- État de survol et de dépôt valide/invalide pour les zones de glisser-déposer (bordure surlignée au survol, retour à la position d'origine en cas d'échec).
- État actif/inactif des boutons de mode ou de proposition QCM.
- Transition visuelle entre deux questions (pas de rechargement brut de page, une transition douce).
- Badge ou indicateur de score à largeur stable pendant le jeu (le contenu numérique change sans provoquer de saut visuel de mise en page).

## Ce qui n'est pas demandé pour cette maquette

- Pas de mode "Explorer" ni de mode "Simuler" façon simulateur d'instrument — cet outil suit le gabarit "jeu de niveaux/séries", pas le gabarit "4 modes" des autres simulateurs du projet.
- Pas de réplique visuelle d'un site commercial existant ; s'inspirer du concept fonctionnel d'applications d'apprentissage ludiques en général, sans copier un design précis.
