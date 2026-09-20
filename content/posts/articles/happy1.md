---
title: Happiness without Purpose
date: 2026-09-20
draft: true
tags: []
description: ""
cover: ""
link: ""
toc: true
math: false
---

Start writing here.

Notes:
- Set `draft: false` when it is ready to publish.
- `description` is what shows on the card and in search results.
- Save the post inside one of: articles / methods / publications / research.

Card thumbnails — the first of these that is filled in wins:
1. `cover: "/img/my-picture.jpg"` — your own image, saved in `static/img/`.
2. `cover: "https://example.org/picture.jpg"` — any image on the web. It is
   downloaded and resized when the site builds, so the page never hot-links.
3. `link: "https://journal.org/the-article"` — for a post that points at
   something published elsewhere. The site reads that page's social preview
   image (`og:image`) and uses it as the thumbnail.
4. Nothing — the card gets a patterned tile with the title's first letter.
   The pattern is chosen from the title, so each post keeps the same one.

`image:` and `thumbnail:` work as aliases for `cover:`.

Publications — you do not need to type the title, authors, journal, year or
abstract. Put the DOI (or the article's URL) in the front matter and they are
fetched when the site builds:

    doi: "10.1186/s12889-025-25811-5"

or, for something without a DOI:

    source: "https://journal.org/articles/12345"

Anything you do fill in yourself wins over what is fetched, so add a `title:`
if you want a shorter one on the card, or a `description:` to replace the
abstract. `new-publication.md` in this folder is a ready-made starting point.
