# CLAUDE.md

## Project Overview

This is a personal CV/resume website for **Pipat Luengnaruemitchai**, Managing Director & Chief Economist at Kiatnakin Phatra Financial Group. The site is a self-contained, single-page application with no build toolchain.

**Tech stack:** Pure HTML5, CSS3, vanilla JavaScript — no frameworks, no bundlers, no package manager.

## Repository Structure

```
cv/
└── index.html   # The entire website: markup, styles, and scripts in one file (~1160 lines)
```

There are no other source files, configuration files, or dependencies beyond Google Fonts loaded via CDN.

## File Layout: `index.html`

The file is organized into three logical blocks:

### 1. `<head>` — Metadata & CSS (lines 1–780)
- Google Fonts import: **Cormorant Garamond** (serif headings), **DM Mono** (monospace labels/buttons), **DM Sans** (body text)
- All styles are inlined in a single `<style>` block
- CSS custom properties (variables) are defined on `:root`
- Responsive breakpoints via `@media (max-width: 768px)` near the end of the style block

### 2. `<body>` — Markup (lines 782–1158)
Sections in page order:

| Section | `id` | Description |
|---|---|---|
| Hero | *(none, `.hero`)* | Name, title, photo, stat cards, awards |
| About | `#about` | Bio text and career highlights grid |
| Experience | `#experience` | Vertical timeline of career positions |
| Education | `#education` | University credentials |
| Awards | `#awards` | Notable recognitions (AsiaMoney etc.) |
| Skills | `#skills` | Tag cloud of professional skills |
| Contact | `#contact` | LinkedIn and company links |
| Footer | *(none)* | Copyright line |

### 3. `<script>` — JavaScript (lines 1134–1158)
Two `IntersectionObserver` instances:
- **Scroll reveal**: adds `.visible` class to `.reveal` elements when they enter viewport
- **Timeline reveal**: staggers `.timeline-item` entries with a 100 ms delay

## Design System

All design tokens are CSS variables on `:root`:

```css
--navy:      #0a1628   /* primary background */
--deep:      #050e1c   /* page background */
--gold:      #c9a84c   /* accent, labels, CTA */
--gold-light: #e8c97a  /* hero name italic, stat numbers */
--cream:     #f5f0e8   /* body text */
--muted:     #8a9ab5   /* secondary text */
--border:    rgba(201, 168, 76, 0.2)   /* card/nav borders */
--glass:     rgba(255, 255, 255, 0.03) /* card background */
```

**Font roles:**
- `Cormorant Garamond` — display headings (`section-title`, `hero-name`, stat numbers)
- `DM Mono` — monospace labels, nav links, buttons, section labels
- `DM Sans` — body copy (default `font-family` on `body`)

**Animation:** CSS `@keyframes fadeUp` and `fadeLeft` used for hero entry animations with staggered `animation-delay` values. Scroll-triggered reveals use the `.reveal` / `.visible` class pattern via `IntersectionObserver`.

## Development Workflow

Since there is no build system, development is purely file-based:

1. **Edit** `index.html` directly in any editor
2. **Preview** by opening `index.html` in a browser (or using a local HTTP server, e.g. `python3 -m http.server 8080`)
3. No compilation, transpilation, linting, or testing steps exist

### Recommended local server

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

Or with Node (if available):

```bash
npx serve .
```

## Conventions to Follow

### HTML
- All section IDs use lowercase kebab-case (`id="experience"`)
- Semantic HTML5 elements (`<section>`, `<nav>`, `<footer>`)
- Comments mark each major block: `<!-- NAV -->`, `<!-- HERO -->`, `<!-- ABOUT -->`, etc.
- Character entities for special characters (`&amp;`, `&nbsp;`)

### CSS
- Class names use lowercase kebab-case (`.timeline-item`, `.stat-card`)
- BEM-lite naming: block prefix + element suffix (`.hero-left`, `.hero-name`, `.nav-links`, `.nav-logo`)
- All spacing uses `rem` units; font sizes use `clamp()` for fluid scaling where appropriate
- Animations are defined once at the end of the `<style>` block and reused via class + delay
- Media queries target `max-width: 768px` for mobile; override grid/flex layouts there

### JavaScript
- Vanilla ES6+ only (no libraries)
- Uses `IntersectionObserver` — do not use scroll event listeners as a replacement
- Keep scripts minimal and at the bottom of `<body>`

### Content Updates
- Career positions live in `.timeline-item` divs inside `#experience`
- Awards are in `.award-card` divs inside `#awards`
- Skills are `.skill-tag` spans inside `#skills`
- Update the footer copyright year when making annual updates

## No Build / No CI

There is no:
- `package.json`, `Makefile`, or any build configuration
- Test suite
- Linter configuration
- CI/CD pipeline
- Deployment scripts

The site is deployed by serving the single `index.html` file statically.

## External Dependencies (CDN only)

| Resource | URL |
|---|---|
| Google Fonts | `https://fonts.googleapis.com/css2?family=Cormorant+Garamond...` |
| LinkedIn profile | `https://www.linkedin.com/in/pipat-luengnaruemitchai-a392b77/` |
| Company site | `https://www.kkpfg.com` |

All other assets (noise texture SVG, profile photo) are inlined directly in the HTML as `data:` URIs or base64 strings.
