# game-sites

Two Hugo sites for RRCHNM, sharing a single theme:

- **games-hub/** — production target `games.rrchnm.org` — RRCHNM Red `#C32A26`
- **plague-site/** — production target `1665plague.rrchnm.org` — GMU Teal `#008285`
- **themes/rrchnm/** — shared theme (Direction D: Broadside body + editorial header)

Brand fonts: Figtree · Noto Serif · Open Sans (GMU brand stack).

---

## Local development

### 1. Install Hugo (extended edition)

| Platform | Command |
|---|---|
| macOS | `brew install hugo` |
| Windows | `winget install Hugo.Hugo.Extended` |
| Linux (Debian/Ubuntu) | Download `.deb` from [Hugo releases](https://github.com/gohugoio/hugo/releases) and `sudo dpkg -i hugo_extended_*.deb` |

Verify: `hugo version` should print `v0.100+ … extended`.

### 2. Run a site

```bash
cd games-hub && hugo server           # → http://localhost:1313
# or, in another terminal:
cd plague-site && hugo server -p 1314 # → http://localhost:1314
```

`hugo server` watches files and live-reloads — edit a template, content file, or CSS rule and the browser refreshes automatically.

### 3. Build for production

```bash
cd games-hub && hugo
cd plague-site && hugo
```

Output goes to each site's `public/` (gitignored).

---

## Project structure

```
.
├── themes/rrchnm/        Shared theme (layouts, partials, CSS)
│   ├── layouts/_default/ baseof.html, single.html
│   ├── layouts/partials/ head, header, footer, rrchnm-mark, orn-rule
│   └── static/css/       style.css (full design system)
│
├── games-hub/            Hub site
│   ├── config.toml       Site name, primary color, menus
│   ├── content/          _index.md, about/_index.md, bibliography/_index.md
│   ├── data/             featured_game, in_development, bibliography, about_sections (YAML)
│   ├── layouts/          Page-level overrides (index, about, bibliography)
│   └── static/images/    Hero & period images
│
├── plague-site/          Plague site
│   ├── config.toml
│   ├── content/          _index.md, pedagogical-materials/_index.md
│   ├── data/             game_details, lessons (YAML)
│   ├── layouts/          index, pedagogical-materials
│   └── static/images/
│
├── landing/              Root index.html for the GitHub Pages preview
└── .github/workflows/    deploy.yml — builds both sites & deploys to Pages
```

## Updating content

Most editable content lives in `data/*.yaml`. Add a bibliography entry, a lesson, or an in-development game by editing the YAML file — no template changes required.

---

## Live preview (GitHub Pages)

Pushed commits to `main` trigger `.github/workflows/deploy.yml`, which builds both sites and deploys to:

- **Landing:** https://jmotis.github.io/game-sites/
- **Hub:** https://jmotis.github.io/game-sites/hub/
- **Plague:** https://jmotis.github.io/game-sites/plague/

`baseURL` is overridden at build time so paths work under the `/game-sites/hub/` and `/game-sites/plague/` subpaths. Production deploys to `games.rrchnm.org` and `1665plague.rrchnm.org` use the values in each `config.toml` instead.

### One-time GitHub Pages setup

In the repo settings → **Pages** → **Build and deployment**, set **Source: GitHub Actions**. (If "Deploy from a branch" was previously selected, switch it.) The first push to `main` after that will trigger the deploy.
