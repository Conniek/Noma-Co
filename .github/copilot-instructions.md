# Instructions projet — Thème Shopify

> À placer dans `.github/copilot-instructions.md` (et copier en `AGENTS.md` à la racine si besoin).
> Compléter les champs [entre crochets].

## Contexte
- Boutique : Noma&Co — Vente d'affiches et toiles d'art digital africain
- Thème de base : Dwell 4.2.0
- Langue du site :  fr + en
- Maquettes de référence : dossier `/maquettes` (images générées). Quand on te donne une maquette, reproduis-la fidèlement en respectant les règles ci-dessous.

## Architecture (Online Store 2.0 uniquement)
- **Snippets** (`snippets/`) : petits composants réutilisables, sans schema (carte produit, bouton, badge, prix, icône). Appelés avec `{% render 'nom', param: valeur %}` — jamais `{% include %}`.
- **Sections** (`sections/`) : blocs de page autonomes, avec un `{% schema %}` complet, modifiables dans l'éditeur de thème.
- **Templates** (`templates/*.json`) : assemblage des sections. Ne pas créer de templates `.liquid`.
- Toujours privilégier les **blocks** dans le schema pour le contenu répétable (slides, témoignages, items de FAQ…).

## Règles de travail
- **Ne jamais modifier les fichiers d'origine du thème.** Toute nouveauté va dans des fichiers préfixés `custom-` (ex. `sections/custom-hero.liquid`, `snippets/custom-product-card.liquid`).
- Si une modification d'un fichier d'origine est vraiment indispensable, le signaler explicitement et la limiter au strict minimum.
- Avant de créer une section, vérifier si le thème en propose déjà une équivalente configurable, et le dire.
- Une section = un fichier autonome : son HTML, son `{% stylesheet %}` ou CSS scopé, son JS éventuel.
- Rien de codé en dur : tout texte, image, lien, couleur ou espacement visible doit être un réglage du schema.

## Schema
- Chaque section a : `name`, `tag`, `class`, `settings`, `blocks` si pertinent, et un `presets` pour être ajoutable depuis l'éditeur.
- Réglages typés correctement : `image_picker`, `richtext`, `url`, `color_scheme` (si le thème l'utilise), `range` pour les espacements, `select` pour les variantes de mise en page.
- Labels et infos des réglages en français, clairs pour une personne non technique.
- Ajouter les réglages d'espacement haut/bas (`padding_top`, `padding_bottom`) sur chaque section.

## CSS
- Réutiliser les variables CSS et classes utilitaires du thème avant d'en créer de nouvelles.
- Styles scopés à la section (`#shopify-section-{{ section.id }}` ou classe propre à la section).
- Mobile first, sans framework CSS externe.

## JavaScript
- JS vanilla uniquement, pas de jQuery ni de librairie externe sans demande explicite.
- Utiliser des custom elements (`<custom-slider>`) comme le reste du thème si c'est sa convention.
- Le contenu doit rester lisible sans JS.

## Performance et éco-conception
- Images : `{{ image | image_url: width: … | image_tag: loading: 'lazy', widths: '…', sizes: '…' }}`, avec `width`/`height` pour éviter les décalages de mise en page. Pas de lazy loading sur l'image principale au-dessus de la ligne de flottaison.
- Pas de vidéo en lecture automatique par défaut ; poster obligatoire.
- Pas de polices supplémentaires hors réglages typographiques du thème.
- Limiter les requêtes et le JS au strict nécessaire.

## Accessibilité (RGAA / WCAG AA)
- HTML sémantique : un seul `h1` par page, hiérarchie de titres logique, `button` pour les actions, `a` pour la navigation.
- `alt` des images : utiliser `image.alt`, prévoir un `alt=""` pour les images décoratives.
- Contrastes AA, focus visible, navigation au clavier complète (sliders, menus, modales).
- `aria-*` uniquement quand le HTML natif ne suffit pas.
- Respecter `prefers-reduced-motion` pour les animations.

## Traductions
- Tout texte d'interface fixe (hors réglages) passe par les fichiers `locales/` : `{{ 'sections.custom_hero.cta' | t }}`.

## Synchronisation avec l'éditeur de thème (priorité absolue)
L'éditeur de thème écrit dans les fichiers JSON du thème en ligne (`templates/*.json`, `sections/*.json`, `config/settings_data.json`). Si `shopify theme dev` est lancé sans synchronisation, les fichiers locaux écrasent ce travail.

- **Avant toute modification** (création, édition ou suppression de fichier, y compris `custom-*`), **demander à l'utilisateur** :
  1. s'il a modifié le paramétrage ou le contenu dans l'éditeur de thème depuis la dernière sauvegarde ;
  2. s'il veut le sauvegarder avant que je commence.
  Ne rien modifier tant qu'il n'a pas répondu.
- **Avant de modifier un `templates/*.json`, un `sections/*.json` ou `config/settings_data.json`** : demander un `shopify theme pull` ciblé (`--only` sur ces fichiers) pour partir de la version de l'éditeur. Ne jamais réécrire ces fichiers à partir d'une version locale potentiellement périmée.
- **Sauvegarde** : proposer un commit git après chaque session d'éditeur, avant de modifier quoi que ce soit.
- **Serveur de dev** : toujours rappeler de lancer `shopify theme dev --theme-editor-sync`, jamais sans l'option.
- **Conflits** : si le terminal propose « local » ou « remote », choisir **remote** (garde le travail fait dans l'éditeur).
- Quand un seul sens de travail est possible à la fois : soit l'utilisateur travaille dans l'éditeur, soit je modifie les fichiers, pas les deux en parallèle.

## Méthode
0. Poser les questions de synchronisation ci-dessus avant toute modification.
1. Annoncer en 2-3 lignes ce qui va être créé (fichiers, réglages principaux).
2. Produire les fichiers complets, prêts à l'emploi.
3. Indiquer comment ajouter la section dans l'éditeur et quoi vérifier dans la preview (`shopify theme dev`).
4. Rappeler de lancer `shopify theme check` si du code a été ajouté.
