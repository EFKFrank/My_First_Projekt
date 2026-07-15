---
description: Génère ou retravaille une présentation HTML façon "slides" dans la charte graphique EFK/CDF (gabarit PowerPoint officiel).
argument-hint: [sujet ou fichier à retravailler] [contenu / plan des slides]
---

Tu vas produire une présentation HTML basée sur **[Reveal.js](https://revealjs.com/)** (moteur de diaporama obligatoire pour toutes les présentations EFK), construite comme une véritable succession de diapositives 16:9, en reprenant fidèlement la charte graphique du gabarit PowerPoint officiel `Präsentationvorlage_EFK.pptx` (thème PowerPoint interne "CH_CDF").

Si `$ARGUMENTS` désigne un fichier existant du dépôt (ex. `training/formation-github.html`), retravaille ce fichier en conservant son contenu pédagogique mais en remplaçant entièrement sa mise en forme par celle décrite ci-dessous. Sinon, `$ARGUMENTS` contient le sujet et/ou le plan de contenu à partir duquel générer une nouvelle présentation. Si le contenu (plan, nombre de slides, messages clés) n'est pas fourni, demande-le avant de générer quoi que ce soit — ne fabrique pas de contenu à la place de l'utilisateur.

## Moteur : Reveal.js

- Charger Reveal.js **via CDN, sur une version figée** (ne jamais utiliser `@latest`) :
  ```html
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/reveal.js@5.1.0/dist/reveal.css">
  <script src="https://cdn.jsdelivr.net/npm/reveal.js@5.1.0/dist/reveal.js"></script>
  ```
  Ne pas charger un thème Reveal.js préexistant (`white.css`, `black.css`, etc.) : partir de `reveal.css` seul, puis appliquer intégralement la charte EFK via un bloc `<style>` propre au fichier (variables CSS ci-dessous).
- Structure standard :
  ```html
  <div class="reveal">
    <div class="slides">
      <section>...</section>
      <section>...</section>
    </div>
  </div>
  <script>
    Reveal.initialize({
      width: 1280,
      height: 720,
      margin: 0,
      center: false,
      hash: true,
      controls: true,
      progress: true,
      slideNumber: 'c/t',
      transition: 'fade'
    });
  </script>
  ```
  `center: false` est **obligatoire** : sans lui, Reveal.js centre verticalement le contenu de chaque slide par défaut, ce qui contredit la règle "titre toujours en haut à gauche" ci-dessous.
- Utiliser les mécanismes natifs de Reveal.js plutôt que de les réinventer : `slideNumber` pour la pagination, la barre de progression intégrée (`Reveal.configure({ progress: true })` si souhaité), `data-transition` par slide si un type de slide a besoin d'une transition différente, et la navigation clavier/tactile déjà fournie par Reveal (ne pas coder de navigation maison).
- Le logo EFK et le fil `Titre | Lieu | Date | Prénom Nom` doivent apparaître sur chaque slide de contenu. Reveal.js redimensionne `.reveal .slides` via une transformation CSS (`scale`) : un élément en `position: fixed` placé en dehors de `.reveal` ne suivrait pas ce zoom et se désaligne au redimensionnement de la fenêtre. Place donc ce logo/footer en `position: absolute` **à l'intérieur de chaque `<section>`** de contenu (il est alors mis à l'échelle avec le reste de la slide) ; ne le mets pas sur les slides de couverture/clôture (qui ont leur propre traitement, voir plus bas).

### Format
- Ratio **16:9**, dimensions de rendu Reveal.js **`width: 1280, height: 720`** (calées sur le ratio du gabarit officiel 13.333 in × 7.5 in).

### Typographie
- Police officielle : **Calibri** (titres ET corps de texte — c'est aussi bien la police "majeure" que "mineure" du thème).
- Pile de fallback web (Calibri n'est pas garantie hors Windows) :
  `font-family: Calibri, Candara, "Segoe UI", Optima, Arial, sans-serif;`
- Corps de texte : 20pt (≈ 1.25rem), interligne 90%.
- Titre de slide : 28pt (≈ 1.75rem), **gras**.
- Sur-titre / "Rubrique" (kicker au-dessus du titre) : 12pt (≈ 0.75rem).
- Pied de page / numéro de page : 18pt (≈ 1.125rem).

### Couleurs (thème "CH_CDF")
```css
:root {
  --efk-text:       #000000; /* dk1 / tx1 — texte principal */
  --efk-bg:         #FFFFFF; /* lt1 / bg1 — fond des slides de contenu */
  --efk-bg-grey:    #D1D3D4; /* bg2 — fond gris de la couverture ET des slides de clôture */
  --efk-grey-dark:  #ADAFB2; /* dk2 */
  --efk-grey-light: #D1D3D4; /* lt2 (identique à bg2) */
  --efk-accent1:    #EA5A4F; /* rouge corail */
  --efk-accent2:    #003399; /* bleu — couleur des titres de la slide de couverture + liens */
  --efk-accent3:    #00A6A3; /* turquoise */
  --efk-accent4:    #FAE03C; /* jaune */
  --efk-accent5:    #F0DAA4; /* beige/sable */
  --efk-accent6:    #ADAFB2; /* gris (identique à dk2) */
}
```
- Fond des slides de **contenu** : blanc uni (`--efk-bg`).
- Fond de la slide de **couverture** et des slides de **clôture** ("Avez-vous des questions ?", "Merci de votre attention") : gris uni (`--efk-bg-grey`), jamais de dégradé ni de texture.
- Titre en **noir** (`--efk-text`) sur les slides de contenu.
- Titre en **bleu** (`--efk-accent2`) sur la slide de couverture/titre et les slides de clôture.
- Les accents (rouge, turquoise, jaune, beige) servent à ponctuer : puces, pictogrammes, petits blocs de mise en valeur, barres de séparation — jamais en aplat de fond plein écran.
- Puces de listes : chevron/flèche (pas de rond plein ni de tiret), en gris ou en couleur d'accent — inspiré des bullets vectoriels du gabarit.

#### Palette étendue (pour graphiques / data-viz)
Le gabarit officiel définit 6 teintes déclinées chacune en 6 nuances (du plus foncé au plus clair). Utilise ces séries pour des séries de graphiques, des dégradés de heatmap, ou des variantes hover/disabled — ne réutilise pas `--efk-accent2` seul pour 6 séries d'un même graphique, prends la nuance adaptée dans la teinte concernée :

| Teinte | Foncé → clair |
|---|---|
| Rouge | `#8C1911` `#D22619` `#EA5A4F` `#F29C95` `#F7BDB9` `#FBDEDC` |
| Bleu | `#00194D` `#002673` `#003399` `#2970FF` `#70A0FF` `#B8CFFF` |
| Turquoise | `#005352` `#007D7A` `#00A6A3` `#7FD0CF` `#BAE3E1` `#D6F5F2` |
| Jaune | `#978304` `#E3C406` `#FAE03C` `#FCEC8A` `#FDF3B1` `#FEF9D8` |
| Sable | `#AD831D` `#E2B74D` `#F0DAA4` `#F6E9C8` `#F9F0DB` `#FCF8ED` |
| Gris | `#55575B` `#808388` `#ADAFB2` `#CECFD1` `#DEDFE0` `#EFEFF0` |

### Assets vectoriels vendorisés
Ces fichiers ont été extraits (vectorisés via potrace depuis les images du gabarit officiel) ou redessinés, et vivent dans `assets/efk/` à la racine du dépôt (chemin à adapter selon l'emplacement du fichier de sortie, ex. `../assets/efk/...` depuis `training/`). Réutilise-les tels quels, ne les régénère pas, et ne les recolore/désature jamais (voir règle de vivacité plus bas) :
- `assets/efk/logo.svg` — le petit logo EFK du bandeau (croix suisse grise + drapeau rouge), vectoriel, fond transparent. Utilisé en haut à droite de **toutes les slides de contenu** (pas la couverture, voir ci-dessous).
- `assets/efk/institution-lockup.png` — le nom de l'institution en 5 langues (DE/FR/IT/RM/EN, texte noir) + le pictogramme croix-blanche/drapeau-rouge en grand, fond transparent. Réservé à la **slide de couverture uniquement**, à la place du petit logo standard.
- `assets/efk/pictograms/*.svg` — les 15 pictogrammes métier vectoriels (traits bleu `#003399` / rouge `#EA5A4F`, fond transparent) : `achats-publics`, `administration-federale`, `armee-protection-population`, `assurances-sociales`, `autres-audits`, `culture-sport`, `economie-agriculture`, `energie-communication`, `finances-publiques-impots`, `formation-recherche`, `justice-police`, `projets-informatiques`, `relations-etranger`, `sante`, `transport-environnement`.
- `assets/efk/icon-questions.svg` — bulles de discussion bleue (avec "?") et rouge, pour la slide "Avez-vous des questions ?".
- `assets/efk/icon-attention-ear-eye.svg` et `assets/efk/icon-attention-smiley.svg` — deux variantes pour la slide "Merci de votre attention" (oreille+œil = plus institutionnel, sourire = plus chaleureux) : choisis celle qui correspond au ton de la présentation, ou demande à l'utilisateur s'il n'y a pas de préférence évidente.

Si un de ces fichiers manque (nouveau dépôt, copie partielle), recrée-le à l'identique avant de continuer plutôt que d'improviser un remplaçant visuellement différent.

**Règle de vivacité** : les pictogrammes et icônes s'affichent toujours en **pleine opacité** (`opacity: 1`), y compris sur fond gris — ne jamais les désaturer/éclaircir pour en faire un "watermark" discret. La discrétion sur la couverture vient de leur taille modeste et de leur positionnement (jamais du fait de les rendre pâles).

### Mise en page (repères issus des positions réelles du gabarit, sur un canevas Reveal.js 1280×720 — 1px = 9525 EMU)
- Marge gauche/droite du bloc de contenu : ~0.88 in (≈ 45px, en pratique le gabarit utilise 90px de marge sur les slides de contenu).
- Logo EFK (`assets/efk/logo.svg`) : en haut à droite de chaque slide de **contenu** (~62×60px) — position officielle, ne pas le mettre à gauche sauf demande explicite.
- Pied de page sur chaque slide de contenu : à gauche `Titre | Lieu | Date | Prénom Nom`, à droite le numéro de slide — texte petit, gris.
- Titre de slide toujours aligné à gauche, jamais centré (sauf sous-titre de couverture, voir plus bas) — y compris sur la couverture et les slides de clôture : le titre est en haut/à gauche, ce n'est plus un bloc centré au milieu de l'écran.

#### Couverture : positions exactes des 15 pictogrammes
Le gabarit officiel positionne les 15 pictogrammes métier de façon précise et non aléatoire sur la couverture "avec icônes" — reproduis ces positions et tailles exactement (en px, canevas 1280×720) :

| Pictogramme | x | y | largeur | hauteur |
|---|---|---|---|---|
| `finances-publiques-impots` | 173 | 300 | 93 | 72 |
| `economie-agriculture` | 404 | 285 | 90 | 87 |
| `formation-recherche` | 652 | 291 | 81 | 81 |
| `sante` | 864 | 294 | 93 | 78 |
| `assurances-sociales` | 1103 | 270 | 81 | 102 |
| `transport-environnement` | 171 | 419 | 93 | 84 |
| `armee-protection-population` | 413 | 413 | 72 | 90 |
| `relations-etranger` | 644 | 407 | 96 | 96 |
| `justice-police` | 859 | 428 | 102 | 75 |
| `projets-informatiques` | 1095 | 428 | 96 | 75 |
| `culture-sport` | 172 | 553 | 93 | 87 |
| `energie-communication` | 396 | 551 | 105 | 90 |
| `achats-publics` | 649 | 553 | 87 | 87 |
| `administration-federale` | 872 | 556 | 75 | 84 |
| `autres-audits` | 1104 | 562 | 78 | 78 |

Ce nuage d'icônes occupe la moitié inférieure de la slide (sous le titre/sous-titre) et quasiment toute la largeur ; ne les recentre pas, ne les redimensionne pas de façon uniforme — chaque taille est légèrement différente dans l'original.

### Types de slides à disposition (calqués sur les masques du gabarit)
1. **Couverture (avec/sans icônes)** — fond gris (`--efk-bg-grey`). Titre + sous-titre en **haut à gauche** (pas centrés verticalement), position ~(84, 68) pour le titre. `assets/efk/institution-lockup.png` en haut à droite (remplace le petit logo standard sur cette slide uniquement). Variante "avec icônes" : les 15 pictogrammes positionnés selon le tableau ci-dessus, en pleine couleur. Pas de pied de page.
2. **Sommaire / Agenda** — titre "Willkommen"-style + liste des sujets à droite.
3. **Titre + contenu** — titre noir en haut, corps de texte/liste à puces en dessous (jusqu'à 3 niveaux d'indentation).
4. **Deux contenus** — deux colonnes de contenu côte à côte sous un même titre.
5. **Comparaison** — deux colonnes avec chacune un sous-titre + contenu (pour comparer deux options/concepts).
6. **Clôture — Questions** — fond gris, titre bleu **en haut à gauche** "Avez-vous des questions ?", icône `assets/efk/icon-questions.svg` **large (≥ 220px) et à droite**, verticalement centrée. Pas de pied de page.
7. **Clôture — Remerciement** — même structure que Questions : titre bleu en haut à gauche "Merci de votre attention", icône (`icon-attention-ear-eye.svg` ou `icon-attention-smiley.svg`) large (≥ 220px) et à droite. Pas de pied de page.

Choisis le type de slide le plus adapté à chaque section du contenu fourni ; ne force pas tout dans un seul type. Une présentation se termine typiquement par les deux slides de clôture (Questions puis Remerciement), dans cet ordre ou l'inverse selon la préférence de l'utilisateur.

## Processus
1. Si le contenu à présenter n'est pas clair ou incomplet, pose la question avant de générer le fichier.
2. Génère/retravaille un seul fichier HTML basé sur Reveal.js (chargé en CDN comme décrit ci-dessus ; seule dépendance externe autorisée), avec la charte EFK appliquée dans une balise `<style>` propre au fichier.
3. Découpe le contenu en slides Reveal.js cohérentes (une idée forte par `<section>`), en s'appuyant sur la pagination et la navigation natives de Reveal.js plutôt que sur du code maison.
4. Garde le nom et l'emplacement du fichier existant si tu retravailles une présentation déjà présente dans le dépôt.
5. Une fois le fichier généré, ouvre-le brièvement (ou décris son plan) pour vérifier que l'enchaînement des slides est cohérent avant de considérer la tâche terminée.
