---
title: "{{title}}"
date: {{date:YYYY-MM-DD}}
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
