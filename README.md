# June Powell — Portfolio Website Handoff

A personal portfolio website, live on GitHub Pages at
`https://juneashpowell-droid.github.io/junepowell/`.

This is a **finished, self-contained static site** — the HTML *is* the product,
not a mockup to reimplement in another framework. Your job in Claude Code is to
**edit the source, re-bundle, and redeploy**.

---

## What's in this package

```
design_handoff_portfolio/
├── README.md            ← you are here
├── src/                 ← EDITABLE SOURCE (edit these)
│   ├── June Powell - Landing.dc.html    ← the cover / landing page
│   ├── June Powell Portfolio.dc.html    ← the main portfolio page
│   ├── support.js                       ← runtime that renders .dc.html (do not edit)
│   └── assets/                          ← all photos + logos
└── dist/                ← DEPLOYABLE FOLDER (Option A below — copy of src/, renamed)
    ├── index.html       ← landing page   (site opens here)
    ├── portfolio.html   ← portfolio page
    ├── support.js
    └── assets/
```

`dist/` used to hold single-file bundles produced by an internal tool not available
in Claude Code (see "Option B" below). Claude Code can't reproduce that exact
gzip+base64 packed format, so `dist/` was regenerated as a plain **Option A**
folder instead: `src/`'s two `.dc.html` files renamed to `index.html` /
`portfolio.html`, plus `support.js` and `assets/` copied alongside them. Deploy
`dist/`'s contents as-is (see "Push to GitHub Pages" below) — no bundler needed.

---

## How these files work ("Design Components")

Each `*.dc.html` file is a full HTML document that loads `support.js` and contains
a single `<x-dc>…</x-dc>` block. `support.js` is a small client-side runtime that
renders that block. You can open any `.dc.html` directly in a browser (with
`support.js` and `assets/` sitting next to it) and it just works — no build step,
no npm, no bundler required for local preview.

Structure inside each file:
- `<helmet>` — fonts, `@keyframes`, `@font-face`, and `<style>` resets.
- Template markup — inline-styled HTML. **All styling is inline** by design; there
  are no CSS classes for layout. Keep it that way when editing.
- A `<script data-dc-script>` `class Component extends DCLogic { … }` block holds any
  JS logic (the landing page uses it for its intro animation + redirect).

### Editing
Edit the markup/copy/styles directly in the `.dc.html` files. To preview locally:
```
cd src
python3 -m http.server 8000
# open http://localhost:8000/June%20Powell%20-%20Landing.dc.html
```

### Key cross-page links (keep these consistent!)
- Landing → portfolio: links point to **`portfolio.html`** (href + a JS redirect
  near the bottom of the landing file: `window.location.href = 'portfolio.html'`).
- Portfolio → cover: the "← Cover" nav link points to **`index.html`**.

If you rename the deployed files, update these links to match.

---

## Deploying (the important part)

GitHub Pages serves plain files. The `.dc.html` sources depend on `support.js` +
`assets/` sitting alongside them, so you have two deploy options:

### Option A — Deploy the folder as-is (simplest for Claude Code)
Commit `support.js`, `assets/`, and the two `.dc.html` files to the repo, renaming:
- `June Powell - Landing.dc.html` → `index.html`
- `June Powell Portfolio.dc.html` → `portfolio.html`

Because they load `./support.js` and `assets/…` by relative path, they render on
Pages as long as those files are in the same repo. Update the internal links to use
the new names (`index.html` / `portfolio.html`).

### Option B — Ship single self-contained files (what's deployed now)
The `dist/` files have `support.js`, every image, and all CSS **inlined into one
HTML file each** — no external dependencies, nothing else to upload. This is how the
site is currently deployed. Regenerating these bundles was done with an internal
tool not available in Claude Code; to reproduce it there, write a small Node script
that inlines `support.js` and base64-encodes each `assets/*` reference into the HTML,
or simply use Option A instead.

### Push to GitHub Pages
```
git add .
git commit -m "Update portfolio"
git push origin main
```
Pages redeploys automatically (Settings → Pages is already set to `main` / root).
Hard-refresh (Cmd+Shift+R) after ~1 min; GitHub caches aggressively.

---

## Design tokens (for reference)

Colors:
- Deep green (ink / dark sections): `#1B3327`, `#16281F`, `#152A20`
- Mid greens (accents / text): `#2C5140`, `#4C6140`, `#33604A`, `#9DB08F`
- Cream / paper background: `#F1ECDF`, `#EDE7D6`, `#E7E0CE`
- Brown (links / labels): `#6E543F`
- Body text on cream: `#3B473F`, `#4A5850`; muted: `#6C7267`

Type:
- Serif display/body: **EB Garamond**
- Sans (labels, nav, UI): **IBM Plex Sans**
- Handwriting accent: **Caveat**

Radius: cards `12–14px`. Max content width: `~1160px`.

---

## Assets
All photos and the GiveGig / Bespoke logos are in `src/assets/`. The two logos
(`givegig-logo.png`, `bespoke-logo.png`) are transparent PNGs. Large photos were
recompressed (~1700px wide, JPEG q0.82) to keep the site light.
