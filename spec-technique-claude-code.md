# Spécifications techniques — Jeu "Unités, multiples et sous-multiples"

Document destiné à Claude Code pour la construction de l'outil. À lire avec le document de design associé (prompt pour Claude Design) et la fiche outil de cours jointe au projet.

---

## 1. Contexte pédagogique

Outil destiné aux élèves de STI2D (2I2D / Terminale) du Lycée Jean Moulin, pour s'entraîner sur les conversions d'unités avec préfixes multiples et sous-multiples, en incluant la notion de puissance de 10. Basé sur la fiche outil "Séquence 01 — Unités, multiples et sous-multiples" (préfixes de giga à nano, conversions simples, surfaces/volumes, grandeurs composées).

Reprend le gabarit fonctionnel du jeu "Formules en jeu" (glisser-déposer, niveaux, étoiles, localStorage), et non celui du simulateur 4 modes (pied à coulisse).

## 2. Format de sortie

Page **HTML/CSS/JS autonome à fichier unique**, sans dépendance externe, utilisable hors-ligne, dépôt direct possible sur ENT/Moodle.

## 3. Architecture générale

### 3.1 Écrans

1. **Écran d'accueil / sélection de bloc** : présente les 4 blocs sous forme de cartes, avec statut (verrouillé / débloqué / meilleur score obtenu). Bloc 1 toujours débloqué. Bloc N+1 débloqué si le bloc N a été validé au moins une fois (score minimal à définir, ex. ≥ 10/20).
2. **Écran de jeu** : une série de 20 questions à enchaîner pour le bloc sélectionné.
3. **Écran de résultats de série** : score, étoiles, message adapté, tableau récapitulatif question par question, boutons "Recommencer ce bloc" et "Retour à l'accueil" (ou "Bloc suivant" si débloqué).

### 3.2 Les 4 blocs

| Bloc | Thème | Contenu couvert |
|---|---|---|
| 1 | Unités simples — écarts faibles | Conversions entre préfixes proches (écart d'exposant ≤ 3), grandeurs de base (g, s, m, A, J, V, W, etc.) |
| 2 | Unités simples — écarts grands | Conversions avec écart d'exposant plus important (> 3, plafonné à 9) ou préfixes moins courants (nano, giga) |
| 3 | Surfaces et volumes | Conversion d'unités au carré ou au cube (piège du facteur élevé au carré/cube), équivalences L/dm³/cm³/mL |
| 4 | Grandeurs composées | Conversion de grandeurs à deux dimensions (ex. L·min⁻¹ → m³·s⁻¹), chaque grandeur convertie séparément puis recomposée |

Chaque bloc = **générateur aléatoire paramétrable**, pas de contenu figé. Une série = 20 questions tirées aléatoirement à chaque lancement, selon les règles du bloc (voir section 5).

### 3.3 Machine à états d'une série

```
accueil → (sélection bloc) → question 1 → ... → question 20 → écran résultats → accueil (ou nouvelle série, ou bloc suivant)
```

Pour chaque question : `idle` (question affichée, pas de réponse) → `answered` (réponse validée, feedback affiché, correction visible) → passage à la question suivante.

## 4. Les 3 formats d'exercice (dont 4 variantes pour le glisser-déposer)

Mélangés aléatoirement au sein d'une même série, dès le bloc 1. Répartition suggérée par tirage aléatoire pondéré (à ajuster en test) : environ 40% direct, 30% QCM coefficient, 30% glisser-déposer.

### 4.1 Format "Conversion directe" (saisie)

- Une valeur avec son unité de départ est affichée (ex. "1500 g").
- Selon le cas : unité cible déjà imposée et affichée ("= ... kg"), ou l'élève doit choisir l'unité cible parmi 3-4 propositions avant de saisir sa valeur.
- Champ de saisie numérique, validation par bouton ou touche Entrée.
- Tolérance de comparaison stricte mais avec epsilon flottant (`+ 1e-9`), et tolérance relative additionnelle pour absorber les arrondis légitimes de l'élève (ex. écart relatif < 0,5%).

### 4.2 Format "QCM coefficient"

- Une conversion est présentée sous forme d'écart entre deux préfixes (ex. "de mA à A, on...").
- Toutes les valeurs s'expriment sous la forme Valeur × 10ⁿ, n pouvant être positif ou négatif : donc 4 propositions du type "× 10³" / "× 10⁻³" / "× 10⁶" / "× 10⁻⁶" (uniquement des multiplications, le signe de l'exposant porte l'information de sens), pour piéger la confusion signalée dans la fiche outil entre 10⁻³ (milli) et 10³ (kilo).
- Une seule bonne réponse, feedback immédiat après clic.

### 4.3 Format "Glisser-déposer"

Quatre variantes possibles au sein de ce format (tirées aléatoirement) :

**Variante A — glisser une valeur vers le bon préfixe/unité cible**
Une valeur numérique convertie est donnée sur une carte déplaçable ; plusieurs préfixes/unités cibles sont proposés comme zones de dépôt (drop zones) ; l'élève doit glisser la valeur vers la zone correspondant à l'unité de départ annoncée. Exemple concret : on annonce "0,053 MJ", plusieurs cartes-valeurs déjà calculées sont présentées (53 000 J / 53 J / 5,3 J...), une seule doit être glissée vers la zone "J".

**Variante B — glisser/positionner la puissance de 10 correspondante**
Une conversion est affichée avec un espace vide à la place de l'exposant (ex. "1500 g = 1,5 × 10 ▢ kg" ou schéma similaire) ; une palette de puissances de 10 (10⁻⁹, 10⁻⁶, 10⁻³, 10⁻², 10⁻¹, 10⁰, 10³, 10⁶, 10⁹, avec quelques leurres proches) est proposée ; l'élève glisse la bonne puissance dans l'emplacement vide.

**Variante C — glisser/positionner le préfixe (multiple ou sous-multiple) correspondant**
Même principe que la variante B, mais l'espace vide porte sur le préfixe lui-même plutôt que sur la puissance de 10 (ex. "1500 g = 1,5 ▢g", l'élève doit glisser le symbole "k" dans l'emplacement vide) ; palette de préfixes disponibles (G, M, k, d, c, m, µ, n, et l'unité de base) avec quelques leurres proches (ex. m et M, c et µ) pour bien travailler la distinction visuelle et le sens des symboles.

**Variante D — mix des variantes A, B et C**
Une même question combine plusieurs éléments vides à compléter par glisser-déposer simultanément (par exemple la valeur numérique ET la puissance de 10, ou le préfixe ET la valeur), chacun avec sa propre zone de dépôt et sa propre palette de propositions. Variante plus exigeante, à réserver de préférence aux blocs 2 à 4 une fois les mécaniques individuelles A, B et C bien maîtrisées séparément dans le bloc 1. Validation soit au fur et à mesure de chaque dépôt, soit globale une fois tous les emplacements remplis (à trancher en test selon la fluidité obtenue).

Interactions : glisser-déposer à la souris, feedback visuel de zone de dépôt valide au survol (bordure surlignée), retour à la position d'origine si le dépôt échoue, validation immédiate au dépôt (pas de bouton "Valider" séparé pour ce format, contrairement aux deux autres).

## 5. Modèle de données du générateur

### 5.1 Table des préfixes de référence

```js
const PREFIXES = [
  { symbole: 'G', nom: 'giga', exposant: 9 },
  { symbole: 'M', nom: 'méga', exposant: 6 },
  { symbole: 'k', nom: 'kilo', exposant: 3 },
  { symbole: '',  nom: 'unité de base', exposant: 0 },
  { symbole: 'd', nom: 'déci', exposant: -1 },
  { symbole: 'c', nom: 'centi', exposant: -2 },
  { symbole: 'm', nom: 'milli', exposant: -3 },
  { symbole: 'µ', nom: 'micro', exposant: -6 },
  { symbole: 'n', nom: 'nano', exposant: -9 }
];
```

### 5.2 Table des grandeurs de base utilisables (bloc 1 et 2)

À constituer en s'appuyant sur la fiche outil et en l'enrichissant de grandeurs SI cohérentes avec le programme STI2D (mécanique, électricité, énergie). Exemples de base : gramme (g), seconde (s), mètre (m), ampère (A), volt (V), watt (W), joule (J), ohm (Ω), hertz (Hz), pascal (Pa), newton (N). Vérifier avec l'enseignant si des grandeurs supplémentaires doivent être exclues ou ajoutées (le contenu exact reste à valider en test).

Pas toutes les grandeurs n'acceptent tous les préfixes en pratique pédagogique courante (ex. on ne dit pas usuellement "gigaseconde"). Prévoir une sous-table de préfixes usuels par grandeur pour éviter de générer des cas absurdes, plutôt qu'un produit cartésien brut grandeur × préfixe.

### 5.3 Génération bloc 1 (écarts faibles)

- Choisir une grandeur, un préfixe de départ, un préfixe cible avec `|écart exposant| ≤ 3` et préfixe départ ≠ préfixe cible.
- Valeur de départ générée aléatoirement mais "propre" (1 à 3 chiffres significatifs, pas de décimale absurde), pour rester lisible.

### 5.4 Génération bloc 2 (écarts grands)

- Même mécanique, mais `écart exposant > 3`, plafonné à 9 pour rester pédagogiquement raisonnable (éviter les cas extrêmes type giga → nano, écart 18, jugés peu réalistes).
- Favoriser l'apparition de préfixes moins courants (µ, n, G) dans ce bloc.

### 5.5 Génération bloc 3 (surfaces / volumes)

- Piocher soit une conversion de surface (unité de longueur élevée au carré : km² ↔ m² ↔ cm² ↔ mm²), soit une conversion de volume (m³ ↔ dm³/L ↔ cm³/mL), avec alternance aléatoire.
- Le facteur de conversion est le facteur de longueur élevé au carré ou au cube — à calculer dynamiquement dans le code, jamais en dur, pour garantir la cohérence (`facteur = 10^(écart × 2)` pour une surface, `10^(écart × 3)` pour un volume).
- Intégrer les équivalences fixes de la fiche (1 m³ = 1000 L, 1 L = 1 dm³, 1 cm³ = 1 mL) comme cas particuliers du générateur, pas comme exceptions séparées non vérifiées.

### 5.6 Génération bloc 4 (grandeurs composées)

- Grandeur composée = ratio de deux grandeurs simples avec unités et préfixes chacune (ex. L·min⁻¹, m·s⁻¹, g·cm⁻³...).
- Générer indépendamment la conversion du numérateur et celle du dénominateur, combiner les deux facteurs pour obtenir le résultat final.
- Prévoir explicitement, dans la génération ou dans les leurres du QCM/glisser-déposer, le piège identifié dans la fiche outil : oublier de convertir l'une des deux grandeurs (souvent le temps) alors que l'autre a été traitée.

### 5.7 Vérification systématique des calculs

Toute formule de génération de facteur de conversion doit être vérifiée par du code exécuté (Node/JS directement dans la logique du jeu, testée en amont) avant intégration — cohérent avec la méthode de travail validée sur le simulateur pied à coulisse. Ne jamais faire confiance à une formule "évidente" sans test unitaire mental ou exécuté.

## 6. Progression, score et sauvegarde

- **Score par série** : nombre de bonnes réponses sur 20.
- **Étoiles** : barème à définir en test, suggestion de départ cohérente avec Formules en jeu (ex. 3 étoiles ≥ 18/20, 2 étoiles ≥ 14/20, 1 étoile ≥ 10/20, sinon 0).
- **Déblocage de bloc** : bloc N+1 débloqué dès que le bloc N atteint le seuil minimal (à définir, ex. 1 étoile).
- **localStorage** : sauvegarder pour chaque bloc le meilleur score obtenu, le nombre d'étoiles, et l'état verrouillé/débloqué. Pas de compte utilisateur, stockage local au poste/navigateur uniquement (cohérent avec l'usage ENT/Moodle en établissement).
- **Écran de résultats** : tableau récapitulatif ligne par ligne (question posée / réponse donnée / réponse correcte / statut), reprenant le principe validé sur le mode Quiz du gabarit simulateur.

## 7. Design et accessibilité (rappel, détaillé dans le document séparé)

- Palette Okabe-Ito (bleu `#0072B2` / orange `#E69F00`) comme palette unique par défaut.
- Police Verdana partout.
- Thème clair/sombre commutable, toutes les couleurs en variables CSS (`cssVar()`), jamais codées en dur.
- Design "moderne, joli, attrayant" comme pour Formules en jeu — voir prompt de design dédié pour le détail visuel.

## 8. Réflexes techniques à appliquer systématiquement

- Vérifier tout calcul ou exemple chiffré par du code exécuté avant intégration.
- Cibler les éléments DOM par `id` explicite, jamais par position.
- Marge epsilon (`+ 1e-9`) pour toute comparaison de tolérance flottante en JavaScript, plus tolérance relative additionnelle pour les réponses saisies (section 4.1).
- Badges/éléments à contenu numérique variable : largeur fixe sur la zone de valeur elle-même, texte aligné à gauche.
- Vérifier que chaque bouton d'un même groupe a bien son gestionnaire d'événement câblé.
- Redessiner tout élément visuel dépendant du thème après un changement de thème.

## 9. Méthode de travail

Construire par blocs courts et testables, avec validation entre chaque étape, dans cet ordre suggéré :

1. Structure statique de l'écran d'accueil (4 cartes de bloc, verrouillage visuel).
2. Générateur du bloc 1 seul (logique pure, testée en console avant intégration visuelle).
3. Écran de jeu avec le format "conversion directe" uniquement, branché sur le générateur du bloc 1.
4. Ajout du format "QCM coefficient".
5. Ajout du format "glisser-déposer" (variantes A et B d'abord, puis C, puis D en dernier une fois les trois autres stables).
6. Écran de résultats de série + système d'étoiles + localStorage.
7. Génération des blocs 2, 3, 4 (un par un, avec vérification des calculs à chaque fois).
8. Déblocage progressif des blocs + persistance globale.
9. Guide d'utilisation et pied de page (mention "Conçu par JB Cocherel — [année]", pas de source d'inspiration externe puisque basé sur la fiche outil interne).
10. Passe finale thème clair/sombre + vérification responsive mobile.

## 10. Points restant à trancher en cours de développement (non bloquants pour démarrer)

- Liste exacte des grandeurs de base retenues par bloc (section 5.2) — à affiner avec JB au fil de la construction du générateur.
- Barème précis des étoiles et seuil de déblocage (section 6) — proposer une valeur par défaut, ajuster après premiers tests.
- Répartition exacte des 3 formats au sein d'une série de 20 (40/30/30 proposé, à valider).
