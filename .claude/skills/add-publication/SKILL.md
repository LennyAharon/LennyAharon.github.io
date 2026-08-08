---
name: add-publication
description: Add or update a publication entry on research.html (Lenny Aharon's personal site), then optionally commit, push to GitHub, and visually verify the live page. Use when the user gives you paper details to add to the site, asks to update a paper's status/venue (e.g. "submitted" -> accepted), or asks to publish/ship/deploy changes to lennyaharon.github.io.
---

# Add a publication to research.html

This site is static HTML served by GitHub Pages directly from the `main` branch of
`LennyAharon/LennyAharon.github.io` — there is no build step. Pushing to `main` is the deploy.

## 1. Gather the entry details

Ask the user for whatever wasn't already given:
- Title
- Authors (bold the self-reference: `<b>Lenny Aharon</b>`)
- Venue / status (e.g. `NeurIPS 2026 (Submitted)`, `ICML 2026 CATS Workshop`, or `[coming soon]` if no venue yet)
- Paper link (arXiv/bioRxiv/OpenReview URL) — omit if not yet available
- Code link (GitHub repo) — omit if not yet available
- BibTeX entry — omit if not yet available
- Image or GIF to pair with the entry (path under `images/`) — if none exists yet, leave the
  `<img>` out rather than inventing a placeholder path

## 2. Build the block

Read `research.html` first. Follow the existing `.paper-entry` pattern exactly — copy the
structure of the nearest existing entry rather than inventing new markup/classes. Shape:

```html
<div class="paper-entry">
	<div class="left-sub-container" style="width: 55%">
		<div class="left-blurb" style="padding-top: 0px; padding-right: 60px">
			<h2>{{TITLE}}</h2>
			<p class="paper-authors">{{AUTHORS}}. {{DATE OR LEAVE OFF}}.</p>
			<p class="paper-links">
				<span class="coming-soon">{{VENUE/STATUS}}</span>
				&nbsp;/&nbsp;
				<a class="doc-link" href="{{PAPER_URL}}">Paper</a>
				&nbsp;/&nbsp;
				<a class="doc-link" href="{{CODE_URL}}">Code</a>
				&nbsp;/&nbsp;
				<a class="doc-link" href="#" onclick="toggleBibtex(event, '{{unique-id}}-bibtex')">bibtex</a>
			</p>
			<pre id="{{unique-id}}-bibtex" class="bibtex-block">{{BIBTEX}}</pre>
		</div>
	</div>
	<div class="right-sub-container" style="width: 42%">
		<div class="center-image">
			<img src="images/{{IMAGE}}" style="border-radius: 4px" />
		</div>
	</div>
</div>
```

Rules:
- `{{unique-id}}` must not collide with any existing bibtex id in the file (grep
  `toggleBibtex(event, '` first to check).
- Drop the Paper/Code/bibtex links (and their `&nbsp;/&nbsp;` separators) individually for
  whichever aren't available yet — don't link to placeholders.
- Ask the user where in the Publications list the new entry should go (the existing order is
  roughly reverse-chronological but not strict) — default to inserting it as the first
  `.paper-entry` right after the `<h2>Publications</h2>` block if they don't have a preference.

## 3. Update sitemap.xml

Bump `<lastmod>` for the `research.html` `<url>` entry in `sitemap.xml` to today's date.

## 4. Review with the user

Show the diff (`git diff`) before doing anything else. Confirm it looks right.

## 5. Commit and offer to push

Commit with a message describing what changed (new publication added / status updated).
Then ask the user whether to push to `origin main` now — GitHub Pages deploys straight from
that push, so pushing **is** publishing to the live site. Don't push without confirmation.

## 6. Verify live

After pushing, GitHub Pages typically republishes within 1–2 minutes (occasionally longer).
If the user wants to see it, use the `claude-in-chrome` skill/tools to navigate to
`https://lennyaharon.github.io/research.html` (or the relevant page) and confirm the new
entry renders as expected — check the bibtex toggle works and the image loads. If it still
shows the old version, wait a bit and reload before assuming something is wrong; you can also
check the repo's Actions tab for the Pages deployment status.
