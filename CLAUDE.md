# Emdash — Landing Page

UAE-based company building Arabic web products. This repo is a single-file static landing page deployed as `index.html`.

## Files

- `index.html` — the live site (committed and deployed)
- `emdash.ae _publish_.html` — the source/export that `index.html` is based on; keep in sync with `index.html`
- `EM-DASH (standalone).html` — earlier standalone version (reference only)

## How the bundle works

All three HTML files are self-contained bundles produced by a custom bundler tool. The structure is:

- `<script type="__bundler/manifest">` — base64-encoded assets (fonts, images) as JSON
- `<script type="__bundler/template">` — the full HTML/CSS/JS as a JSON-escaped string
- A loader script at the top unpacks assets and replaces the document

The actual site HTML/CSS lives **inside the JSON-encoded template string**, which means:
- All content is on effectively one long line
- Edits must be done via string replacement (PowerShell `.Replace()` or similar)
- The CSS uses `html[data-lang="en"]` and `html[data-lang="ar"]` selectors for bilingual EN/AR switching

## Language system

- `html[data-lang="en"]` — English mode (LTR, Bodoni Moda serif)
- `html[data-lang="ar"]` — Arabic mode (RTL, Reem Kufi + IBM Plex Sans Arabic)
- Bilingual elements use `data-en` / `data-ar` attributes (hidden/shown via CSS)
- Product names use `.en` / `.ar` class spans inside `h3` (both always rendered, styled differently)

## Key CSS customisations (applied on top of the publish export)

```css
/* Reduce English product name size to visually match Arabic (Reem Kufi) */
html[data-lang="en"] .product h3 { font-size: clamp(34px, 5.3vw, 72px); }

/* Use logical padding so hover effect mirrors correctly in both LTR and RTL */
.product:hover { padding-inline-end: clamp(20px, 4vw, 48px); }
```

## Deployment

Push to `main` branch. The site is served from `index.html` at the repo root.

## Editing workflow

1. Make CSS/HTML changes via PowerShell string replacement on the file
2. Verify in browser (use the visual companion server or open directly)
3. Apply the same change to both `index.html` and `emdash.ae _publish_.html`
4. Commit and push
