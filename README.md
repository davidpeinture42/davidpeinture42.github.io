# davidpeinture42.github.io

Site vitrine statique pour David NEYRON, plâtrier peintre auto-entrepreneur basé à Firminy (42), intervenant sur Saint-Étienne et le bassin stéphanois.

MVP V1 : HTML + CSS uniquement, zéro JS, zéro dépendance externe, zéro build. Prêt à être déposé sur la branche `main` pour GitHub Pages.

## Structure du dépôt

```
/
├── index.html
├── prestations.html
├── interventions.html
├── contact.html
├── mentions-legales.html
├── zones.html
├── sitemap.xml
├── robots.txt
├── README.md
├── css/
│   └── style.css
└── images/
    ├── favicon.svg
    ├── hero.svg
    └── chantiers/
        ├── chantier-01/
        │   ├── avant.svg
        │   └── apres.svg
        └── chantier-02/
            ├── avant.svg
            └── apres.svg
```

## État des images (important)

Toutes les images sont actuellement des **placeholders SVG neutres** (rectangle gris avec une légende), en attendant les vraies photos de chantiers. Comme un SVG n'a pas de variante AVIF/WebP, les `<img>` du MVP pointent directement sur le `.svg`, sans bloc `<picture>` multi-format.

**Quand une vraie photo est disponible**, elle doit suivre le format cible défini dans le brief : AVIF en source principale, WebP en repli, JPG en dernier recours, toujours via `<picture>`. Voir le template plus bas.

## Template de carte chantier (galerie avant/après)

Ce template est la structure de référence pour `interventions.html`. La duplication entre cartes est assumée : pas de composant JS, pas de boucle, chaque carte est un bloc HTML autonome.

```html
<!--
  Ajouter un nouveau chantier :
  1. Créer le dossier images/chantiers/chantier-0N/
  2. Y déposer avant.avif / avant.webp / avant.jpg et apres.avif / apres.webp / apres.jpg
  3. Copier le bloc ci-dessous dans interventions.html, juste avant </ul>
  4. Adapter le titre, la localisation, les alt et la description
-->
<li class="card">
  <p class="eyebrow eyebrow--onLight">Chantier</p>
  <h2 class="card__title">Titre du chantier <span class="card__location">— Commune (42)</span></h2>
  <div class="card__compare">
    <figure class="card__figure">
      <picture>
        <source srcset="images/chantiers/chantier-0N/avant.avif" type="image/avif">
        <source srcset="images/chantiers/chantier-0N/avant.webp" type="image/webp">
        <img src="images/chantiers/chantier-0N/avant.jpg" alt="Description précise de la pièce avant travaux" width="800" height="600" loading="lazy">
      </picture>
      <figcaption class="card__figcaption">Avant</figcaption>
    </figure>
    <figure class="card__figure">
      <picture>
        <source srcset="images/chantiers/chantier-0N/apres.avif" type="image/avif">
        <source srcset="images/chantiers/chantier-0N/apres.webp" type="image/webp">
        <img src="images/chantiers/chantier-0N/apres.jpg" alt="Description précise de la pièce après travaux" width="800" height="600" loading="lazy">
      </picture>
      <figcaption class="card__figcaption">Après</figcaption>
    </figure>
  </div>
  <p class="card__description">Description courte des travaux réalisés.</p>
</li>
```

Pendant la phase placeholder (SVG), le même bloc s'écrit sans `<picture>` multi-source :

```html
<li class="card">
  <p class="eyebrow eyebrow--onLight">Chantier</p>
  <h2 class="card__title">Titre du chantier <span class="card__location">— Commune (42)</span></h2>
  <div class="card__compare">
    <figure class="card__figure">
      <img src="images/chantiers/chantier-0N/avant.svg" alt="Description précise de la pièce avant travaux" width="800" height="600" loading="lazy">
      <figcaption class="card__figcaption">Avant</figcaption>
    </figure>
    <figure class="card__figure">
      <img src="images/chantiers/chantier-0N/apres.svg" alt="Description précise de la pièce après travaux" width="800" height="600" loading="lazy">
      <figcaption class="card__figcaption">Après</figcaption>
    </figure>
  </div>
  <p class="card__description">Description courte des travaux réalisés.</p>
</li>
```

## Conventions de code

- BEM pour les classes CSS (`card__title`, `card__title--variant`)
- camelCase pour les noms de classes composés en JS si du JS est introduit en V2 (aucun JS en V1)
- Indentation : 2 espaces
- Un seul fichier CSS global : `css/style.css`
- Zéro commentaire dans le HTML/CSS livré (le code se lit seul) ; les commentaires du template ci-dessus sont une exception documentaire propre au README

## Système de design (refonte V1.1)

Trois couleurs brutes seulement, toujours en custom properties :

```css
--color-bg: #FFFFFF;
--color-main: #1B2A4A;
--color-accent: #C9A84C;
```

Tous les autres tokens sont **dérivés** de ces trois couleurs via `color-mix()`, pas des couleurs supplémentaires ajoutées à la main :

```css
--color-surface: color-mix(in srgb, var(--color-main) 5%, white);  /* fonds de bandeaux et de cartes */
--color-border: color-mix(in srgb, var(--color-main) 18%, white);  /* bordures fines des cartes */
--color-muted: color-mix(in srgb, var(--color-main) 65%, white);   /* texte secondaire sur fond clair, ≈4,6:1 */
```

`color-mix()` est supporté par toutes les versions actuelles de Chrome, Firefox, Safari et Edge depuis 2023 — sans risque vu la cible "2 dernières versions, pas d'IE11".

### Règle de contraste sur l'accent

`#C9A84C` sur blanc ≈ 2,3:1 (sous le seuil AA). `#C9A84C` sur `--color-main` ≈ 6,2:1 (conforme). Donc :

- l'accent sert de **texte** uniquement sur fond `--color-main` (bandeaux héro/footer, classe `.eyebrow`)
- sur fond clair, le texte de mise en avant utilise `.eyebrow--onLight` (`--color-muted`, ≈4,6:1) au lieu de l'accent
- l'accent reste le fond des `.btn` (texte `--color-main` dessus, ≈6,2:1)

### Pattern de bandeaux pleine largeur

Le héro et le footer sont les seuls éléments toujours en pleine largeur colorée (`--color-main`). Les sections de contenu alternent :

- sections "plates" : une seule balise porte `class="section wrapper"` (fond = fond de page, pas de structure supplémentaire)
- sections "teintées" : structure en deux niveaux, `<section class="section section--tint">` (pleine largeur, fond `--color-surface`) contenant un `<div class="wrapper">` qui recentre le contenu

### Cartes (`.tile`)

`valueList__item`, `testimonial`, `zoneList__item` et `contactList__item` partagent désormais le même habillage visuel via la classe utilitaire `.tile` (fond `--color-surface`, bordure `--color-border`, `--radius-lg`), combinée à leur classe BEM spécifique pour le contenu. La galerie de chantiers (`.card`) suit la même logique visuelle mais garde ses propres règles, son contenu (paire avant/après) étant structurellement différent.



## Mise à jour de l'hébergement

GitHub Pages sert le contenu de la branche `main` à la racine. Aucune étape de build n'est nécessaire : chaque fichier de ce dépôt est déposé tel quel.

## À compléter avant mise en ligne réelle

`mentions-legales.html` contient deux sections avec des champs `[À COMPLÉTER]`, obligatoires pour un artisan du bâtiment et non inventables par un tiers :

- **Assurance professionnelle** — nom et adresse de l'assureur (RC pro / garantie décennale), zone de couverture géographique. Obligation légale : article L. 243-2 du Code des assurances.
- **Médiation de la consommation** — nom et coordonnées du médiateur de la consommation désigné. Obligation légale : articles L. 612-1 et suivants du Code de la consommation. Si aucun médiateur n'est encore désigné, David NEYRON doit s'affilier à un médiateur agréé (liste sur le site de la Commission d'évaluation et de contrôle de la médiation de la consommation, CECMC) avant publication.

Tant que ces champs ne sont pas remplis avec les vraies informations, le site ne doit pas être considéré comme conforme pour une mise en ligne publique définitive.

## V2 (hors scope du MVP)

- JS vanilla : lightbox sur la galerie, menu mobile si besoin
- Minification du CSS
- Remplacement des SVG placeholders par les vraies photos (AVIF/WebP/JPG)
- Formulaire de contact si un jour un besoin de saisie utilisateur apparaît (impliquerait alors une réflexion sécurité : validation, anti-spam, etc.)
