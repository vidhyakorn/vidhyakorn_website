# vma

Personal research site, published at https://vidhyakorn.github.io/vma/ — history and philosophy of science and medicine,
epidemiological modelling, vaccine policy and health surveillance.

Built with [Hugo](https://gohugo.io) and the
[Spectra](https://github.com/JoeYang1412/hugo-theme-spectra) theme.

## Requirements

Hugo **Extended** 0.157.0 or newer — the theme compiles SCSS, which the
non-extended build cannot do.

## Working locally

```bash
git clone https://github.com/vidhyakorn/vma.git
cd vma
hugo server -D
```

No submodule step. The theme is committed to this repo as ordinary files
under `themes/hugo-theme-spectra/`, so a plain clone is complete.

## Writing

Categories come from the folder, not from front matter. A post in
`content/posts/methods/` appears under Methods in the sidebar.

```
content/posts/
├── _index.md
├── articles/
├── methods/
├── publications/
└── research/
```

Front matter Spectra reads:

```yaml
---
title: "Post title"
date: 2026-01-01
draft: false
tags: [tag one, tag two]
series: [series name]   # optional; adds previous/next navigation
description: "Shown on the card and in meta tags."
cover: "cover.webp"     # optional
toc: true
math: false             # set true to load KaTeX on this page
---
```

For a cover image, put the file beside the post — or make the post a folder
with `index.md` plus the image — and Hugo will crop and compress it.

## Updating the theme

The theme is vendored rather than tracked as a submodule, so updating is a
replace-and-commit:

```bash
git clone --depth 1 https://github.com/JoeYang1412/hugo-theme-spectra /tmp/spectra
rm -rf themes/hugo-theme-spectra
mkdir -p themes/hugo-theme-spectra
cp -r /tmp/spectra/. themes/hugo-theme-spectra/
rm -rf themes/hugo-theme-spectra/.git themes/hugo-theme-spectra/.github
git add -A themes && git commit -m "Update Spectra theme"
```

Check `themes/hugo-theme-spectra/README.md` for any new configuration
options before pushing.

## Deploying

Pushing to `main` triggers `.github/workflows/hugo.yml`, which builds with
Hugo Extended and publishes to GitHub Pages. In the repository settings,
Pages → Source must be set to **GitHub Actions**.
