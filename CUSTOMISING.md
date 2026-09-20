# Changing how the site looks

Everything you are likely to want to change lives in two files:

| What | File |
|---|---|
| Colours, fonts, site title, menu, social links | `hugo.toml` |
| Card size, spacing, anything finer | `assets\css\custom.css` |

`themes\hugo-theme-spectra\` is the theme. **Never edit anything in there.** If
you change a theme file, the next theme update overwrites it. Everything in
`layouts\` at the top level is a copy that overrides the theme's version, and
those are safe to edit.

After any change: save, commit and push in GitHub Desktop. The site rebuilds by
itself and is live in two or three minutes.

---

## Colours

In `hugo.toml`, under `[params.style]`:

```toml
[params.style]
  colorAccent = '#1A2CA3'      # links, active menu item, buttons
  colorAccentWarm = '#CD071F'  # small marks — tag hover, the dot on each card
  colorAccentSoft = '#F68048'  # decorative fills only, never text
  bgPrimary = '#FFFFFF'        # page background
  bgSurface = '#FFFFFF'        # card and panel background
  textPrimary = '#141A2E'      # body text
```

The current palette, and why each colour sits where it does:

| Colour | Used for | Contrast on white |
|---|---|---|
| `#0D1A63` Ink | headings | 15.6 : 1 |
| `#1A2CA3` Deep | links, active tab | 10.9 : 1 |
| `#2845D6` Bright | hover, focus, fills | 7.2 : 1 |
| `#CD071F` Signal | small features | 5.8 : 1 |
| `#F68048` Flame | fills and marks only | **2.6 : 1 — fails** |

Text needs at least 4.5 : 1 to be readable for everyone. Flame is nowhere near,
which is why it is never used for words — only as a fill, and anything sitting
*on* it is dark blue. If you swap in a new colour, check it at
[webaim.org/resources/contrastchecker](https://webaim.org/resources/contrastchecker/)
before using it for text.

Headings use a colour that is **not** in that config block, because the theme
has no setting for it. It is the `--ink` line near the top of `custom.css`:

```css
:root {
  --ink: #0D1A63;
}
```

Dark mode has its own set of values further down `custom.css`, under
`[data-theme="dark"]`. The dark blues are lifted there — `#1A2CA3` is invisible
against a dark background.

---

## Fonts

Two steps, both in `hugo.toml`, and you must do both.

**1. Load the font.** Go to [fonts.google.com](https://fonts.google.com), pick
your fonts, copy the URL out of the embed snippet, and replace this line:

```toml
googleFontsURL = 'https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400..700;1,400..700&family=IM+Fell+English:ital@0;1&family=JetBrains+Mono:wght@400;500&display=swap'
```

This one line replaces *every* font the site loads, so whatever you put there
must include all three — heading, body and the monospace one used for code and
dates. Drop `JetBrains+Mono` and the code blocks lose their font.

**2. Use it.** Under `[params.style]`:

```toml
fontDisplay = "'IM Fell English', Georgia, 'Times New Roman', serif"
fontBody = "'EB Garamond', Georgia, 'Times New Roman', serif"
fontMono = "'JetBrains Mono', ui-monospace, SFMono-Regular, monospace"
```

`fontDisplay` is the site title, banner, headings and card titles. `fontBody` is
everything you read. The names after the first are fallbacks, used while the
font downloads or if it fails — keep at least one.

**A warning about weights.** IM Fell English only comes in regular and italic.
Ask a browser for bold and it fakes one by smearing the letters, which looks
bad. That is why `custom.css` forces headings to `font-weight: 400`. If you move
to a font that *does* have a bold, delete this block from `custom.css` to get
bold headings back:

```css
h1, h2, h3, h4, h5, h6,
.article-header h1,
.post-card-title,
.banner-title {
  font-weight: 400;
}
```

**Body text size** — EB Garamond sets small, so it is nudged up. In
`custom.css`:

```css
.single-layout > .article-content p { font-size: 1.06rem; }
```

`1rem` is the browser default, roughly 16px. `1.06rem` is 6% larger.

---

## Cards

All in `custom.css`, in the section headed `CARDS`.

**Height.** One number controls every card:

```css
@media (min-width: 901px) {
  :root { --card-h: 230px; }
}
```

Every card is exactly this tall whether or not it has an image, which is what
keeps a list of posts even. Make it smaller and the text clamps tighter; make it
much smaller and the title starts to get cut off.

**Image width:**

```css
.post-card-cover { width: 240px; }
```

The image is cropped to fill that box, so a tall photo and a wide screenshot
take the same space.

**How much text shows.** The title is cut off after two lines and the summary
after two:

```css
.post-card h3 { -webkit-line-clamp: 2; }
.post-card p  { -webkit-line-clamp: 2; }
```

Raise those numbers and you must raise `--card-h` too, or the extra lines are
simply hidden.

**Summary length** — in `hugo.toml`, `cardSummaryLength = 150` is how many
characters of the description are used before the `…`.

**On phones** cards stack with the image on top, so the *image* gets a fixed
height instead of the whole card. That is the `@media (max-width: 900px)` block
just below.

Everything inside `@media (min-width: 901px)` applies on laptops and desktops
only; everything inside `@media (max-width: 900px)` applies on phones. **This is
the usual reason a change appears to do nothing** — if you edit a rule in the
desktop block and then look at the site in a narrow window, it will not apply.

---

## Spacing

In `custom.css`, under `SPACING`:

```css
.main-content  { padding: 2rem 1.75rem; }   /* margin around the whole page */
.article-content { padding: 2rem 2.25rem; } /* inside an article */
.post-card { margin-bottom: 1.1rem; }       /* gap between cards */
.banner { height: 34vh; }                   /* the coloured header */
```

`vh` means percent of screen height, so `34vh` is about a third of the window.

---

## The banner

In `hugo.toml`:

```toml
bannerTitle = 'Vidhyakorn Mahd-Adam'
bannerSubtitle = 'A scientific approach applied through …'
showBanner = true            # false removes it entirely
bannerImageGradient = 'linear-gradient(135deg, #0D1A63 0%, #2845D6 55%, #F68048 100%)'
```

To use a photo instead of the gradient, put the file in `static\img\` and set
`banner = '/img/yourfile.jpg'` under `[params]`.

---

## Menu, sidebar, profile

In `hugo.toml`:

- `title`, `tagline`, `bio`, `avatar` — the top of the sidebar
- `[[menu.main]]` blocks — the navigation. `weight` sets the order (lower first),
  `pre` is the little symbol
- `[[params.social]]` blocks — the icons under your photo
- `[params.sidebar]` — which sections appear, and how many items each shows

---

## Adding a post

Write it in Obsidian, anywhere under `content\posts\`. **The folder decides the
category**: a file in `content\posts\articles\` appears under Articles. There is
no category setting in the file itself.

```yaml
---
title: "Your title"
date: 2026-09-20
draft: true          # false to publish
tags: [modelling, covid-19]
description: "One or two sentences — this is the card text."
cover: "/img/yourpicture.jpg"
---
```

**A file with no front matter still gets published as a blank page.** If you
create a note and leave it empty, delete it rather than leaving it in
`content\`.

### Card images

First one filled in wins:

1. `cover: "/img/picture.jpg"` — your own file, saved in `static\img\`. The path
   starts at `/img/`, not `/static/img/`.
2. `cover: "https://example.org/picture.jpg"` — any image on the web, downloaded
   and resized at build time.
3. `link: "https://journal.org/article"` — the site reads that page's preview
   image.
4. Nothing — a patterned tile with the title's first letter, the pattern chosen
   from the title so it never changes.

### Putting a diagram in the text

**The simple way.** Save the file in `static\img\`, then in the post write:

```markdown
![What the diagram shows](/img/model-structure.png)
```

The path starts at `/img/`, never `/static/img/` — `static` is the folder Hugo
copies *from*, so it disappears from the address. The text in the brackets is
what a screen reader announces and what shows if the image fails to load; it is
worth writing properly.

That stretches the picture to the full width of the column, which is right for a
screenshot and usually too big for a diagram. To size it, add a caption, or
wrap text around it, use the figure shortcode instead:

```markdown
{{< figure src="/img/model-structure.png"
           alt="Compartments and the flows between them"
           caption="**Figure 1.** Structure of the model."
           width="520" >}}
```

- `width` is in pixels and caps the picture; leave it out for full width.
- `caption` accepts markdown, so `**Figure 1.**` comes out bold.
- `align="left"` or `align="right"` floats it and lets the text wrap around;
  the default is centred. On a phone it always goes full width.

Readers can click any picture in an article to see it full size.

**Pasting straight from Obsidian.** Turn the post into a *folder* instead of a
single file — `hanami2026.md` becomes `hanami2026\index.md`. Images can then
sit in that folder beside the post and you refer to them by name alone:

```markdown
![Model structure](diagram.png)

{{< figure src="diagram.png" caption="Figure 1." width="520" >}}
```

This is what makes pasting work. Obsidian is already set to save attachments
next to the note, so a pasted screenshot lands in the right folder on its own.
One setting is worth changing: **Settings → Files & Links → New link format →
Relative to note**. Without it Obsidian writes a long path from the vault root,
which Hugo cannot follow.

Keeping each post in its own folder also keeps its pictures with it — move or
delete the post and the images go too.

**Two things that will not work.** Obsidian's own embed syntax, `![[file.png]]`,
is not markdown and Hugo ignores it; use `![](file.png)`. And an image simply
dropped next to an ordinary `.md` post — one that is not a folder with
`index.md` — is not published at all, so the picture comes out broken.

### Publications — no typing

Put the DOI in the front matter and leave the rest out:

```yaml
---
date: 2026-09-20
doi: "10.1186/s12889-025-25811-5"
tags: [modelling]
---
```

The title, all the authors, the journal, the year and the abstract are fetched
when the site builds, and a citation panel with a "Read the paper" button is
added under the heading. Journal DOIs come from Crossref; figshare, Zenodo and
other repository DOIs come from DataCite. For something with no DOI use
`source: "https://..."` and its page's citation tags are read instead.

Anything you write yourself overrides what is fetched — add a `title:` for a
shorter one on the card, or a `description:` in place of the abstract.

---

## When a change seems to do nothing

1. **Check the width.** Rules inside `@media (min-width: 901px)` do not apply to
   a narrow window. Maximise the browser and look again.
2. **Wait for the build.** Pushing is not publishing. Open the repository on
   GitHub, click **Actions**, and wait for the tick. Two or three minutes.
3. **Hard refresh** — Ctrl+F5. The stylesheet is now named after a hash of its
   contents, so a change renames the file and the browser is forced to fetch it.
   Pages themselves can still be held in cache for a minute or two.
4. **Check you edited the right file.** A change in
   `themes\hugo-theme-spectra\` is overridden by the copy in `layouts\` or by
   `custom.css`, and will appear to do nothing.

## Seeing changes instantly

Instead of pushing and waiting each time, run the site on your own machine:

```
cd D:\1project\vidhyakorn_website
hugo server
```

Open http://localhost:1313. Every save appears in the browser immediately. Stop
it with Ctrl+C. You need Hugo **extended** installed for this — the site uses
SCSS, which the ordinary build cannot compile.
