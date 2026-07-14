---
description: Génère ou retravaille une présentation HTML façon "slides" dans la charte graphique EFK/CDF (gabarit PowerPoint officiel).
argument-hint: [sujet ou fichier à retravailler] [contenu / plan des slides]
---

Tu vas produire une présentation au format **HTML autonome** (un seul fichier, sans dépendance externe), construite comme une véritable succession de diapositives 16:9, en reprenant fidèlement la charte graphique du gabarit PowerPoint officiel `Präsentationvorlage_EFK.pptx` (thème PowerPoint interne "CH_CDF").

Si `$ARGUMENTS` désigne un fichier existant du dépôt (ex. `training/formation-github.html`), retravaille ce fichier en conservant son contenu pédagogique mais en remplaçant entièrement sa mise en forme par celle décrite ci-dessous. Sinon, `$ARGUMENTS` contient le sujet et/ou le plan de contenu à partir duquel générer une nouvelle présentation. Si le contenu (plan, nombre de slides, messages clés) n'est pas fourni, demande-le avant de générer quoi que ce soit — ne fabrique pas de contenu à la place de l'utilisateur.

## Gabarit de référence (extrait du .pptx officiel)

### Format
- Ratio **16:9**, page de 12 192 000 × 6 858 000 EMU = **13.333 in × 7.5 in**.
- En HTML : conteneur `.slide` en `aspect-ratio: 16 / 9`, largeur max ~1280px, centré à l'écran, une slide affichée à la fois (navigation clavier ← → + boutons, comme un vrai diaporama).

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
  --efk-bg:         #FFFFFF; /* lt1 / bg1 — fond, toujours blanc */
  --efk-grey-dark:  #ADAFB2; /* dk2 */
  --efk-grey-light: #D1D3D4; /* lt2 */
  --efk-accent1:    #EA5A4F; /* rouge corail */
  --efk-accent2:    #003399; /* bleu — couleur des titres de la slide de couverture + liens */
  --efk-accent3:    #00A6A3; /* turquoise */
  --efk-accent4:    #FAE03C; /* jaune */
  --efk-accent5:    #F0DAA4; /* beige/sable */
  --efk-accent6:    #ADAFB2; /* gris (identique à dk2) */
}
```
- Fond de toutes les slides : **blanc uni** (`--efk-bg`), pas de dégradé ni de texture.
- Titre en **noir** (`--efk-text`) sur les slides de contenu.
- Titre en **bleu** (`--efk-accent2`) uniquement sur la slide de couverture/titre.
- Les accents (rouge, turquoise, jaune, beige) servent à ponctuer : puces, pictogrammes, petits blocs de mise en valeur, barres de séparation — jamais en aplat de fond plein écran.
- Puces de listes : chevron/flèche (pas de rond plein ni de tiret), en gris ou en couleur d'accent — inspiré des bullets vectoriels du gabarit.

### Mise en page (repères issus des positions réelles du gabarit)
- Marge gauche/droite du bloc de contenu : ~0.88 in (≈ 45px sur une slide de 1280px de large).
- Logo EFK : petit logo en haut à droite de chaque slide (~0.65in × 0.62in), zone réservée même si tu n'as pas le fichier de logo (laisser un espace ou un simple monogramme texte "EFK").
- Pied de page sur chaque slide de contenu : à gauche `Titre | Lieu | Date | Prénom Nom`, à droite le numéro de slide — texte petit, gris.
- Titre de slide toujours aligné à gauche, jamais centré (sauf slide de couverture).

### Types de slides à disposition (calqués sur les masques du gabarit)
1. **Couverture (avec/sans icônes)** — titre bleu centré verticalement dans le tiers supérieur, sous-titre en dessous, pas de pied de page.
2. **Sommaire / Agenda** — titre "Willkommen"-style + liste des sujets à droite.
3. **Titre + contenu** — titre noir en haut, corps de texte/liste à puces en dessous (jusqu'à 3 niveaux d'indentation).
4. **Deux contenus** — deux colonnes de contenu côte à côte sous un même titre.
5. **Comparaison** — deux colonnes avec chacune un sous-titre + contenu (pour comparer deux options/concepts).
6. **Slide de remerciement / questions** — texte centré, grand, peu d'éléments ("Avez-vous des questions ?", "Merci de votre attention").

Choisis le type de slide le plus adapté à chaque section du contenu fourni ; ne force pas tout dans un seul type.

## Processus
1. Si le contenu à présenter n'est pas clair ou incomplet, pose la question avant de générer le fichier.
2. Génère/retravaille un seul fichier HTML autonome (CSS inline dans une balise `<style>`, pas de dépendance CDN).
3. Découpe le contenu en slides cohérentes (une idée forte par slide), avec navigation clavier/boutons et indicateur "slide x / n".
4. Garde le nom et l'emplacement du fichier existant si tu retravailles une présentation déjà présente dans le dépôt.
5. Une fois le fichier généré, ouvre-le brièvement (ou décris son plan) pour vérifier que l'enchaînement des slides est cohérent avant de considérer la tâche terminée.
