# Convertix

Jeu éducatif web pour s'entraîner aux conversions d'unités avec préfixes (multiples et
sous-multiples) et puissances de 10, à destination des lycéens en STI2D (2I2D / Terminale).

4 blocs de difficulté croissante (unités simples à écarts faibles, unités simples à écarts grands,
surfaces et volumes, grandeurs composées), un générateur aléatoire paramétrable (jamais de contenu
figé), 3 formats d'exercice dont 4 variantes de glisser-déposer, un système de 5 étoiles et une
progression sauvegardée localement.

Le jeu existe en **deux versions distinctes**, qui partagent exactement le même générateur et les
mêmes règles, mais pas la même interaction :

| Version | Fichier | Interaction | Public visé |
|---|---|---|---|
| PC | [`index.html`](index.html) | Glissé-déposé (souris) | Ordinateur, en classe ou à la maison |
| Mobile | [`index-mobile.html`](index-mobile.html) | Tap-to-place (toucher un élément, puis toucher la case) | Smartphone / tablette Android ou iOS |

## Jouer en ligne

- **Version PC** : https://jbcocherel-s2i.github.io/convertix/
- **Version mobile** : https://jbcocherel-s2i.github.io/convertix/index-mobile.html

## Essayer le jeu en local

Aucune installation, aucune dépendance, aucune étape de build : ce sont des pages HTML autonomes.

- **Version PC** : ouvrir [`index.html`](index.html) dans un navigateur.
- **Version mobile** : ouvrir [`index-mobile.html`](index-mobile.html) (fonctionne aussi bien en
ouverture directe du fichier que servi en HTTP).

## Fonctionnalités

- 4 blocs de difficulté (écarts faibles, écarts grands, surfaces/volumes, grandeurs composées),
générés aléatoirement à chaque série de 20 questions — jamais de contenu figé.
- 3 formats d'exercice mélangés aléatoirement : conversion directe (saisie, avec bascule
automatique en écriture scientifique mantisse × 10^exposant au-delà de 3 zéros), QCM coefficient
(× 10ⁿ), et glisser-déposer en 4 variantes (valeur, puissance de 10, préfixe, ou mix des deux).
- Pièges pédagogiques ciblés : confusion kilo/milli dans les QCM, préfixes visuellement proches
(m/M, c/µ) dans le glisser-déposer, oubli de conversion d'une grandeur dans les grandeurs
composées.
- Cas non décimaux traités à part (tr/min ↔ tr/s, m³/s ↔ L/min, km/h ↔ m/s, débit massique en
min/h) : toujours en saisie directe, jamais en QCM ou glisser-déposer.
- Système de 5 étoiles selon le score sur 20, déblocage du bloc suivant à partir de 4 étoiles.
- Progression sauvegardée en `localStorage` (meilleur score, étoiles, blocs débloqués par
appareil/navigateur), avec bouton de réinitialisation dans le guide intégré.
- Thème clair/sombre, guide d'utilisation intégré ("Comment jouer ?").

## Structure du dépôt

```
.
├── index.html                              Version PC (glissé-déposé souris)
├── index-mobile.html                       Version mobile (tap-to-place tactile)
├── spec-technique-claude-code.md           Spécifications techniques complètes
├── template-simulateur-pedagogique.md      Document de design (direction visuelle)
└── README.md
```

Contrairement à un projet multi-fichiers, chaque version est **intégralement autonome** : pas de
fichier JS partagé, toute la logique (générateurs, formats, sauvegarde) est dupliquée à l'identique
dans les deux fichiers. Choix délibéré, conforme au cahier des charges, pour que chaque page reste
déposable telle quelle sur un ENT/Moodle sans dépendance ni étape de build.

## Documentation

- [Spécifications techniques](spec-technique-claude-code.md) — architecture, 4 blocs de
difficulté, modèle de données du générateur, formats d'exercice, progression/score/sauvegarde.
- [Document de design](template-simulateur-pedagogique.md) — écrans, direction visuelle (palette
Okabe-Ito, thème clair/sombre).

## Stack technique

Aucune dépendance externe, aucun framework, aucune étape de build : HTML/CSS/JavaScript vanilla.
Choix délibéré pour rester des pages autonomes déposables telles quelles sur un ENT/Moodle, sans
connexion internet requise.

## Licence

Aucune licence définie pour l'instant (tous droits réservés par défaut).

## Auteur

JB Cocherel
