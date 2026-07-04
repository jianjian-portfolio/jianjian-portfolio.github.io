# Jian Jian | Portfolio

Source for [jianjian-portfolio.github.io](https://jianjian-portfolio.github.io/) — a bilingual (English / 中文) personal portfolio built with [Hugo](https://gohugo.io/) and the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme.

**Repository:** https://github.com/jianjian-portfolio/jianjian-portfolio.github.io  
**Default branch:** `master`  
**Live site:** https://jianjian-portfolio.github.io

---

## What this site contains

| Section | EN route | 中文 route | Source |
|---------|----------|------------|--------|
| About | `/` (alias `/about/`) | `/zh/` (alias `/zh/about/`) | `content/_index.md`, `content/_index.zh.md` |
| Projects | `/projects/` | `/zh/projects/` | `content/projects/` (20 bilingual case studies) |
| Career Journey | `/timeline/` (aliases `/certificate/`, `/certificates/`) | `/zh/timeline/` (aliases `/zh/certificate/`, `/zh/certificates/`) | `content/timeline.en.md`, `content/timeline.zh.md` |

**About** — biography, Nature Portfolio publication, NetApp patents, open-source tools, and personal sections.

**Projects** — 20 case studies as paired `*.en.md` / `*.zh.md` files. The list page splits them into:

- **Featured** (`featured: true`, 4 projects): Tobit / Nature research, DeepCell, DRL decision engine, NetApp ARP
- **Enterprise & legacy** (remaining 16 projects): earlier enterprise and research work

**Career Journey** — a chronological career timeline with expandable proof documents, plus a credentials section (degrees, certificates, transcripts).

---

## Tech stack

- Hugo **Extended** (SCSS pipeline for PaperMod)
- PaperMod via **git submodule** (`themes/PaperMod`)
- Custom layouts under `layouts/` (project list/grid, figure shortcodes, SEO partial)
- Site CSS override: `assets/css/extended/custom.css`
- Static files: `static/` (images, favicons, `robots.txt`)
- Google Analytics (`G-92344ML5YY`) via `hugo.toml`
- KaTeX loaded on pages with `math: true` in frontmatter
- GitHub Actions → GitHub Pages (`.github/workflows/hugo-pages.yml`)

---

## Repository layout

```
.github/workflows/hugo-pages.yml   CI: Hugo build + Pages deploy
content/
  _index.md, _index.zh.md          About page (EN / 中文)
  timeline.en.md, timeline.zh.md   Career Journey + credentials
  projects/                        40 project pages + 2 section indexes
layouts/
  _default/single.html             Default single-page layout
  _partials/                       header, cover, extend_head, seo, …
  projects/list.html, single.html  Project index and detail pages
  shortcodes/                      figure-block, figure-stack, figure-grid, …
assets/css/extended/custom.css     PaperMod style extensions
static/
  robots.txt                       Crawler rules + sitemap URL
  assets/images/                   Photos, project images, certificates, career path proofs
  assets/favicon/                  Favicons and PWA icons
themes/PaperMod/                   Theme submodule (see .gitmodules)
hugo.toml                          Site config (languages, menus, SEO, analytics)
.gitmodules                        Submodule definition
```

**Not committed** (see `.gitignore`): `public/`, `.hugo_build.lock`, `resources/_gen/`, `.tmp/`, IDE files.

---

## Local development

**Requirements:** Hugo Extended, Git with submodule support.

```bash
git clone --recurse-submodules https://github.com/jianjian-portfolio/jianjian-portfolio.github.io.git
cd jianjian-portfolio.github.io

# If the repo was cloned without submodules:
git submodule update --init --recursive

hugo server
```

- English: http://localhost:1313/
- 中文: http://localhost:1313/zh/

Production build:

```bash
hugo --minify
```

Output goes to `public/` (regenerated locally; do not commit).

---

## Deployment

Pushes to **`master`** trigger `.github/workflows/hugo-pages.yml`, which:

1. Checks out the repo with submodules (`submodules: recursive`)
2. Installs Hugo Extended and runs `hugo --minify`
3. Uploads `public/` and deploys to GitHub Pages

**One-time GitHub setting:** **Settings → Pages → Build and deployment → Source → GitHub Actions**

---

## Content conventions

- **Bilingual pages:** separate files per language (`*.en.md`, `*.zh.md`), not inline translation blocks.
- **SEO descriptions:** each page has a `description` field in frontmatter for `<meta name="description">`; it is not rendered in the page body.
- **Project frontmatter:** typical fields include `title`, `description`, `date`, `featured`, `weight`, `cover`, and optional `math` or `aliases`.
- **Navigation and languages:** configured in `hugo.toml` under `[languages.en]` and `[languages.zh]`.

---

## Copyright

© 2026 Jian Jian. All rights reserved.

This repository and site content (text, images, certificates, career path materials) are personal portfolio materials. No permission is granted to copy, republish, or reuse them without prior written consent.

**Third-party components:** [PaperMod](https://github.com/adityatelange/hugo-PaperMod) is used as a git submodule under its own [MIT license](https://github.com/adityatelange/hugo-PaperMod/blob/master/LICENSE). That license applies to the theme only, not to this site's content.
