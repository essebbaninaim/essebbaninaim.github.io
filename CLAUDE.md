# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Vue d'ensemble

Portfolio académique personnel de Naïm Es-Sebbani (chercheur IA/ML, CRIL). Site **statique mono-fichier** : tout le contenu, le style et la logique vivent dans `index.html`. Déployé sur **GitHub Pages** via le dépôt `essebbaninaim/essebbaninaim.github.io` (la branche `main` est servie telle quelle).

Il n'y a **aucun build, aucune dépendance npm, aucun test**. Tailwind est chargé par CDN (`cdn.tailwindcss.com`) et les polices via Google Fonts. Pour prévisualiser, ouvrir `index.html` dans un navigateur ou servir le dossier (`python -m http.server`). Pour déployer : `git push` sur `main`.

## Structure du contenu

- `index.html` — page unique. Organisée en sections `<section id="...">` : `parcours`, `competences`, `enseignement`, `publications`, `colloques`, `contact`. La nav (`#navbar`) et le menu mobile (`#mobileMenu`) pointent vers ces ancres.
- `CV.pdf` — CV téléchargeable, lié depuis le hero et le footer.
- `poster/*.pdf` — posters de conférences (`ICML_26`, `ECAI_25`, `ACL_25`, `LREC_26`), affichés en aperçu via `<embed type="application/pdf">` avec `#toolbar=0&navpanes=0&scrollbar=0&view=FitH`.
- `moi.png`, `moi_hero.png` — photos de profil.

## Architecture de `index.html`

Le fichier est découpé en trois blocs :
1. **`<head>`** : config Tailwind inline (`tailwind.config`) définissant la palette `navy` (50→950) et les polices `sans` (Plus Jakarta Sans) / `display` (Playfair Display). Suivi d'un `<style>` avec les animations CSS maison.
2. **`<body>`** : le markup des sections.
3. **`<script>`** finaux : l'i18n et les interactions.

### Internationalisation (FR / EN) — point central

Le bilinguisme est le mécanisme le plus important à comprendre avant toute édition de contenu.

- Tout texte traduisible porte un attribut `data-i18n="cle.sous_cle"`.
- L'objet `i18n = { fr: {...}, en: {...} }` (dans le dernier `<script>`) mappe chaque clé vers son texte. Les valeurs peuvent contenir du HTML (injecté via `innerHTML`, ex. `<br/>`, `<span class=...>`).
- `setLang(lang)` parcourt tous les `[data-i18n]` et remplace leur `innerHTML` selon le dictionnaire ; met aussi à jour `document.title` (clé `meta.title`), le drapeau/code de langue, et persiste le choix dans `localStorage` (clé `lang`, défaut `fr`).

**Règle impérative** : tout texte visible ajouté ou modifié doit (a) porter un `data-i18n`, et (b) avoir une entrée **dans `i18n.fr` ET `i18n.en`**. Le texte écrit en dur dans le HTML n'est qu'un fallback affiché avant l'exécution du JS — il sera écrasé par le dictionnaire. Garder les deux langues synchronisées.

### Animations au scroll

Les classes `.reveal`, `.reveal-left`, `.reveal-right`, `.reveal-scale` démarrent invisibles ; un `IntersectionObserver` ajoute `.visible` à l'entrée dans le viewport. La classe `.stagger` sur un parent décale ses enfants. Pour qu'un nouvel élément s'anime, lui donner une de ces classes (l'observer les sélectionne tous au chargement).

## Conventions

- Langue par défaut FR ; accents et caractères spéciaux obligatoires partout (y compris dans `i18n.en` quand pertinent).
- Style Tailwind utilitaire inline, palette `navy-*`, fond clair (le design est volontairement épuré/académique, pas un thème sombre tape-à-l'œil).
- Dates et libellés de conférences/publications sont dans le dictionnaire i18n, pas en dur dans le markup.
