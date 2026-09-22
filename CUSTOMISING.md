# Changing how the site looks

The site uses the [Hextra](https://imfing.github.io/hextra/docs/) theme, laid
out after [developers.osuny.org](https://developers.osuny.org/) (which is itself
built on Hextra). Nearly everything you will want to change lives in two files:

| What | File |
|---|---|
| Homepage text, menu, icons, site title | `hugo.toml` |
| Colours, card look, spacing, anything finer | `assets\css\custom.css` |

`themes\hextra\` is the theme. **Never edit anything in there** — a theme update
would overwrite it. Everything in `layouts\` at the top level is a copy that
overrides the theme's version, and those are safe to edit.

After any change: save, commit and push in GitHub Desktop. The site rebuilds by
itself and is live in two or three minutes.

---

## Where things live

```
content\
  _index.md                  the homepage (its text is in hugo.toml, see below)
  research\                  everything in the left-hand sidebar
    _index.md                the Research overview page
    publications\            one .md per paper
    projects\                ongoing research
    methods\                 methods notes
  blog\                      Writing — essays, dated, newest first
  about\index.md             About, with your photo and CV
static\img\                  pictures
static\cv\vidhyakorn-cv.pdf  the CV shown on About
```

**The folder decides where a page appears.** A file in
`content\research\projects\` shows up in the sidebar under Projects; a file in
`content\blog\` shows up under Writing. There is no category setting inside the
file.

To add a new sidebar group, make a folder under `content\research\` with an
`_index.md` in it:

```yaml
---
title: "Teaching"
description: "Courses and workshops."
weight: 4          # position in the sidebar: 1 is first
---
```

---

## The homepage

In `hugo.toml`, under `[params.home]`:

```toml
[params.home]
  badge = 'MORU · Bangkok'           # the small pill at the top, with the red dot
  badgeLink = '/about'
  headline = 'τὸ τέλος τοῦ βαλάνου ἐστὶ τὸ γενέσθαι δρῦς'
  subtitle = '“An acorn’s telos is to become an oak tree.” — Aristotle'
  intro = 'I work on …'              # one short paragraph
  buttonText = 'Read my research'
  buttonLink = '/research'
  publications = 3                   # how many cards in each grid
  writing = 3
```

Leave any line out and that piece simply isn't shown. The card grids under the
button — latest publications, latest writing — fill themselves in from your
content; you never edit them. Anything you write in `content\_index.md` appears
below the grids.

---

## Colours

At the top of `assets\css\custom.css`:

```css
:root {
  --primary-hue: 232deg;
  --primary-saturation: 72.5%;
  --primary-lightness: 41.2%;

  --ink: #0D1A63;      /* headings */
  --deep: #1A2CA3;     /* links, buttons */
  --bright: #2845D6;   /* hover */
  --signal: #CD071F;   /* small marks: the badge dot, "Abstract" labels */
  --flame: #F68048;    /* decorative only: card hover line, citation panel edge */
}
```

The three `--primary-*` numbers are how Hextra works: it builds its entire blue
scale — buttons, the active sidebar item, focus rings — from one hue,
saturation and lightness. Its buttons use lightness × 0.9, which is why 41.2%
comes out as exactly `#1A2CA3`. To move to a different main colour, find its
HSL values (any colour picker shows them) and divide the lightness by 0.9.

Contrast on white, for reference — text needs at least 4.5 : 1:

| Colour | Contrast | Safe for text? |
|---|---|---|
| `#0D1A63` Ink | 15.6 : 1 | yes |
| `#1A2CA3` Deep | 10.9 : 1 | yes |
| `#2845D6` Bright | 7.2 : 1 | yes |
| `#CD071F` Signal | 5.8 : 1 | yes |
| `#F68048` Flame | 2.6 : 1 | **no** — fills and lines only |

Dark mode has its own values just below, under `.dark { … }`. The dark blues
are lifted there, because `#1A2CA3` disappears against a black background.

---

## Fonts

The site uses each reader's own system font — San Francisco on a Mac, Segoe UI
on Windows — exactly as Osuny does. Nothing is downloaded, so text appears
instantly.

To use a web font instead, add the Google Fonts `<link>` to
`layouts\_partials\custom\head-end.html` (create it) and set it in
`custom.css`:

```css
body { font-family: 'Inter', ui-sans-serif, system-ui, sans-serif; }
```

---

## Cards

**Pictures** — first one filled in wins:

1. `cover: "/img/picture.jpg"` — a file in `static\img\`. The path starts at
   `/img/`, never `/static/img/`.
2. `cover: "https://example.org/picture.jpg"` — any image on the web.
3. `link: "https://journal.org/article"` — the site uses that page's preview
   image.
4. Nothing — a coloured tile with the title's first letter. The colours are
   chosen from the page's address, so each post keeps the same tile.

Every picture is cropped to the same 16 : 9 box and converted to a small WebP
when the site builds, so a 300 KB photo arrives as a few kilobytes and a grid
stays even. If a path is wrong, the card falls back to the tile and the build
log names the file it couldn't find.

**Shape and size** — in `custom.css`, the `CARDS` section:

```css
.hextra-card .hextra-card-image { aspect-ratio: 16 / 9; }   /* picture shape */
.hextra-card .hextra-card-icon  { -webkit-line-clamp: 3; }  /* title lines */
```

**Card text** — a publication's card shows where and when it appeared, taken
from its DOI. Anything else shows its `description:`, or the start of the text.

---

## Publications — no typing

In `content\research\publications\`, a new file needs only this:

```yaml
---
date: 2026-09-22
doi: "10.1186/s12889-025-25811-5"
tags: [modelling]
---
```

When the site builds, the title, every author, the journal, the year and the
abstract are fetched and put on the page, with "Read the paper" and DOI
buttons. The title also appears in the sidebar, the card and the browser tab.
Journal DOIs come from Crossref; figshare, Zenodo and other repository DOIs from
DataCite. With no DOI, use `source: "https://…"` and the page's own citation
tags are read instead.

Anything you write yourself wins over what is fetched:

- `title:` — a different title everywhere
- `linkTitle:` — a short label for the sidebar only, e.g. `"PHPIT, 2025"`
- `description:` — replaces the fetched line under the card
- text in the body — replaces the fetched abstract on the page

`content\_templates\new-publication.md` is a ready-made starting point.

---

## Pictures and diagrams in the text

```markdown
![What the diagram shows](/img/model-structure.png)
```

That fills the column. To size it or add a caption:

```markdown
{{< figure src="/img/model-structure.png"
           alt="Compartments and the flows between them"
           caption="**Figure 1.** Structure of the model."
           width="520" >}}
```

`width` is in pixels; `align="left"` or `"right"` wraps the text around it.
**Separate the settings with spaces, not commas** — a comma stops the whole site
from building.

To paste from Obsidian, make the post a folder (`hanami2026\index.md`) and keep
its pictures beside it; then `![](diagram.png)` needs no path. Obsidian's own
`![[diagram.png]]` does not work.

---

## Menu and icons

The `[[menu.main]]` blocks in `hugo.toml`, left to right by `weight`. An entry
with an `icon` shows as an icon:

```toml
[[menu.main]]
  name = 'ORCID'
  url = 'https://orcid.org/0009-0008-2468-8870'
  weight = 5
  [menu.main.params]
    icon = 'orcid'
```

Hextra has icons for GitHub, LinkedIn, X, Mastodon, Bluesky and more. ORCID and
Google Scholar were added in `data\icons.yaml`; add others there the same way.

`[[menu.sidebar]]` blocks add links to the bottom of the Research sidebar.

---

## About page

Your photo is `content\about\profile.jpg`. Its size on the page is set in
`custom.css` under `ABOUT PAGE` — change `200px`. The CV comes from
`static\cv\vidhyakorn-cv.pdf`; export a new one from Typst over the top of it.

---

## When a change seems to do nothing

1. **Wait for the build.** Pushing is not publishing. On GitHub, open
   **Actions** and wait for the green tick.
2. **Check the build didn't fail.** A red cross means the live site is still
   the previous version. Click it — the error names the file and the line.
3. **Hard refresh** with Ctrl+F5. The stylesheet is named after a hash of its
   contents, so it can't be stale, but a page can be cached for a minute.
4. **Check you edited the right file** — never a file under `themes\`.

## Seeing changes instantly

Instead of pushing and waiting, run the site on your own machine:

```
cd D:\1project\vidhyakorn_website
hugo server
```

Open http://localhost:1313/vma/ and every save shows up at once. Stop it with
Ctrl+C. Publication details still need an internet connection to be fetched.
