# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Source for Lenny Aharon's personal site, served by GitHub Pages at https://lennyaharon.github.io. Plain static HTML / CSS — no build step, no package manager, no JS framework. Site structure adapted with permission from themattinthehatt.github.io.

## Development

There is no build/lint/test tooling. To preview locally, just open the HTML files directly in a browser, or serve the directory statically, e.g.:

```
python3 -m http.server
```

Changes go live by pushing to the branch GitHub Pages is configured to serve (`main`).

## Structure

- `index.html` — "about" page (bio, education, positions)
- `research.html` — "research" page (research interests, publications, presentations, invited talks, teaching)
- `art.html` — "outside" page
- `css/stylesheet.css` — single shared stylesheet for all pages
- `images/` — all images and video/gif assets referenced by the pages
- `robots.txt`, `sitemap.xml` — update `sitemap.xml`'s `<lastmod>` when a page's content changes meaningfully

Each page is a self-contained HTML file with its own `<head>` (meta tags, Open Graph/Twitter cards, and on `index.html` a JSON-LD `Person` schema block). Page-specific CSS overrides live in an inline `<style>` block in that page's `<head>` rather than in `css/stylesheet.css`.

## Conventions

- Layout uses a shared `centered` / `container` / `left-sub-container` / `right-sub-container` flex pattern (see `css/stylesheet.css`) with inline `width` percentages set per-section in the HTML.
- Nav is duplicated at the top of every page: `about` / `research` / `outside` links to `index.html`, `research.html`, `art.html`.
- Publications in `research.html` follow a repeated `.paper-entry` block: title (`h2`), `.paper-authors` (bold self-name), abstract text, `.paper-links` (venue via `.coming-soon` span when not yet published, then Paper / Code / bibtex links separated by `&nbsp;/&nbsp;`), a hidden `.bibtex-block` `<pre>` toggled via `toggleBibtex(event, '<id>')`, and a paired image/gif on the right. New publications should follow this exact block structure and be added in reverse-chronological order.
- `toggleBibtex` is defined inline in `research.html`; each bibtex block needs a unique `id` matching its toggle link's argument.

## Skills

- `add-publication` (`.claude/skills/add-publication/`) — use this for adding/updating a publication entry on `research.html`; it also handles the commit/push/live-verify flow, since pushing to `main` is what deploys to GitHub Pages.
