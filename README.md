# davidpeinture42.github.io

Site vitrine statique pour David NEYRON, plâtrier peintre auto-entrepreneur basé à Firminy (42), intervenant sur Saint-Étienne et le bassin stéphanois.

MVP V1 : HTML + CSS uniquement, zéro JS, zéro dépendance externe, zéro build. Prêt à être déposé sur la branche `main` pour GitHub Pages.

## Structure du dépôt

```
/
├── index.html
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

## Palette et contraste (à respecter en cas d'ajout de styles)

```css
--color-bg: #FFFFFF;
--color-main: #1B2A4A;
--color-accent: #C9A84C;
```

L'accent `#C9A84C` sur fond blanc donne un ratio de contraste d'environ **2,3:1**, sous le seuil WCAG AA (4,5:1 pour le texte, 3:1 pour les éléments non-textuels comme un focus ring). Il est donc réservé :

- aux fonds de bouton, toujours avec du texte `--color-main` dessus (ratio ≈ 6,2:1, conforme AA)
- aux éléments décoratifs non porteurs de sens (bordures, puces)

Le texte de lien reste en `--color-main` (souligné), jamais en `--color-accent` seul sur fond blanc.

## Mise à jour de l'hébergement

GitHub Pages sert le contenu de la branche `main` à la racine. Aucune étape de build n'est nécessaire : chaque fichier de ce dépôt est déposé tel quel.

## V2 (hors scope du MVP)

- JS vanilla : lightbox sur la galerie, menu mobile si besoin
- Minification du CSS
- Remplacement des SVG placeholders par les vraies photos (AVIF/WebP/JPG)
- Formulaire de contact si un jour un besoin de saisie utilisateur apparaît (impliquerait alors une réflexion sécurité : validation, anti-spam, etc.)
