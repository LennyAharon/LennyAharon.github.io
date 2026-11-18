---
name: sync-from-resume
description: Bring lennyaharon.github.io in line with Lenny's latest resume PDF — publications (order, venues, Oral/Spotlight badges, links, figures), positions, education, honors, talks and teaching. Proposes a change list, previews it, and requires Lenny's explicit approval before committing and again before pushing (pushing to main deploys the live site). Use when Lenny shares an updated resume or says the site is out of date.
---

# Sync the website from the resume

The resume (kept in Overleaf, exported as PDF) is the source of truth. The site is plain static
HTML on GitHub Pages, deployed by pushing to `main`. Nothing is committed or pushed without
Lenny's explicit approval.

## 1. Read the resume, including its links

Extract the text *and* the hyperlinks — paper links live only in the PDF annotations:

```
pdftotext -layout resume.pdf -
python3 -c "import pypdf,sys; r=pypdf.PdfReader(sys.argv[1]); [print(i,[round(x) for x in a.get_object()['/Rect']],a.get_object()['/A']['/URI']) for i,p in enumerate(r.pages) for a in (p.get('/Annots') or []) if a.get_object().get('/A',{}).get('/URI')]" resume.pdf
```

Match each link to its publication by its y-position on the page (titles are linked).

## 2. Diff against the site and build a change list

Compare section by section and write the list out for Lenny before editing:

| Resume section | Site location |
|---|---|
| Publications (order matters) | `research.html` → `.paper-entry` blocks, same order as the resume |
| Experience | `index.html` → Positions (keep site-only older roles unless told otherwise) |
| Education | `index.html` → Education |
| Honors & Programs | `index.html` → Honors & Programs |
| Invited Talks & Workshops | `research.html` → Invited Talks & Workshops |
| Teaching | `research.html` → Teaching |
| Summary | `index.html` bio + `<meta>` / Open Graph / Twitter descriptions + JSON-LD |

Things that drift and must be checked every time: "Present" end dates, venue status
(submitted → accepted, in revision → published), Oral/Spotlight recognition, links that were
`[coming soon]`, bibtex entries copied between papers, and the contact links (LinkedIn, Scholar).

## 3. Publication conventions

- Follow the `.paper-entry` structure described in `CLAUDE.md` / the `add-publication` skill.
- Venue goes in `<span class="coming-soon">` (or a link to the venue page). Recognition goes
  right after it as a badge: `<span class="badge badge-oral">Oral</span>` or
  `<span class="badge badge-spotlight">Spotlight</span>` (styles live in `research.html`'s
  inline `<style>`).
- Each paper gets a figure on the right. Prefer Figure 1 or the teaser figure from the paper,
  exported to `images/<short-name>.png` (≤ ~600 KB) or a short `.mp4`. Videos must use
  `<video src=… autoplay loop muted playsinline>`, never `<img>`. If no figure exists yet, ask
  Lenny for one rather than inserting a placeholder.
- Every bibtex id must be unique and every bibtex must describe its own paper.

## 4. Preview and get approval

Bump `<lastmod>` in `sitemap.xml` for changed pages. Then render the changed pages
(`python3 -m http.server` + a browser, or headless Chrome screenshots) and show Lenny the
screenshots together with the change list. **Stop and wait for explicit approval.**

## 5. Commit and push

Only after approval: commit authored as Lenny (`Lenny Aharon <la2872@columbia.edu>`) with a
message summarising the sync. Ask separately before `git push origin main` — that is the deploy.
After pushing, check https://lennyaharon.github.io in a browser after 1–2 minutes.
