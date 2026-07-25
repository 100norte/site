# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

The marketing website for 100Norte (100Norte, Lda., Espinho, Portugal) — a software & technology house working across AI, .NET/MAUI, renewable energies, automation and networks. It is a hand-written static site: no build step, no package manager, no dependencies, no tests. Editing HTML/CSS in place *is* the workflow.

## Commands

Nothing to build or compile. To preview locally, serve the directory over HTTP:

```bash
python -m http.server 8000
```

## Deployment

GitHub Pages serves `main` of `github.com/100norte/site` directly from the repository root. `CNAME` binds it to `100norte.pt`. A push to `main` is a production deploy — there is no staging environment and no CI.

## Structure

- `index.html` — the entire site. One page, anchor-navigated (`#top`, `#services`, `#work`, `#about`, `#contact`, `#legal`, plus `#privacy`/`#cookies`/`#terms`).
- `styles.css` — the only stylesheet. Brand tokens in `:root`, then components, then two responsive breakpoints at the bottom.
- `assets/` — brand SVGs (logo/logomark/icon in navy, white, red, primary) and `favicon.svg`.
- `services.html`, `about.html`, `projects.html` — meta-refresh stubs redirecting to the matching anchor. They exist only so URLs from the previous multi-page site don't 404; they carry `noindex`. Don't add content to them.

## Design system — Modernist

The visual language comes from a "Modernist" brand system: flat, architectural, set entirely in **Archivo** (400/600/800, loaded from Google Fonts). The rules that actually constrain edits:

- **Zero corner radius anywhere.** No `border-radius`, ever.
- **Strong 2px ink rules**, never hairlines, never replaced by whitespace. The `--rule` token carries this.
- **Everything is flush left**, including labels inside full-width buttons (`.form .btn` uses `space-between` so the label stays at the left padding edge).
- **The accent is used sparingly** — primary action, section numbers, small emphasis. The one place red runs as a full field is the `#contact` band.
- Grids show their structure: `.cell-grid` sets a 2px ink background so the *gaps between white cells* read as rules. That is how the services grid, the stat row and the capability strip are drawn — there are no per-cell borders.

Colors, type and layout all come from `:root` tokens (`--ink`, `--slate`, `--steel`, `--accent`, `--shell`, `--gutter`, `--rule`, …). Take values from there rather than hard-coding a hex.

## Conventions

- **Icons are a vendored inline SVG sprite** at the top of `<body>` — Lucide paths, referenced as `<svg class="icon"><use href="#i-name"/></svg>`. There is no icon library at runtime. To add an icon, copy its paths from [lucide.dev](https://lucide.dev) into a new `<symbol id="i-...">`; size it by setting `width`/`height` on `.icon` in a component rule, and let `stroke: currentColor` inherit the color.
- **Styling lives in `styles.css`, not in the markup.** The page has no inline `style` attributes (the sprite's positioning aside) — add a class instead.
- **Responsive** is two breakpoints, both in the block at the end of `styles.css`: 1024px (multi-column grids halve, hero and contact stack) and 720px (everything becomes one column, `--gutter` tightens). New multi-column layouts need entries in both.
- **JavaScript is one small IIFE at the end of `index.html`** covering three things: cookie-consent persistence, opening a legal `<details>` when linked to by hash, and the contact-form fetch. Anything beyond that probably belongs in CSS.
- The legal accordions are native `<details>`/`<summary>` — no JS toggling, no ARIA bolt-ons.

## External services

- **Formspree** handles contact-form delivery (`https://formspree.io/f/mjgrkzre`). There is no backend. The script POSTs over `fetch` so the visitor stays on the page; the `<form action>` is a real endpoint, so submission still works without JS. Changing that URL breaks contact delivery.
- **Google Fonts** serves Archivo. It is the only third-party runtime request.

## Open items

- The Terms & Conditions panel in `index.html` still contains the literal placeholder `[comarca]` for the competent jurisdiction. It needs the real one filled in before it means anything.
- There is no `og:image`. Social shares render without a preview card image until a raster (PNG/JPG) one is added — SVG will not work for this.
