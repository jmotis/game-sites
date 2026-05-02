# game-sites

Hugo sites for RRCHNM games, sharing a single theme:

- **`games-hub/`** → production target `games.rrchnm.org` — RRCHNM Red `#C32A26`
- **`plague-site/`** → production target `1665plague.rrchnm.org` — GMU Teal `#008285` (Pantone 321C)
- **`shipping-site/`** → production target `1812shipping.rrchnm.org` — GMU Navy `#004F71` (Pantone 653C)
- **`themes/rrchnm/`** — shared theme. The sites differ only in colors, content, and which page layouts they override.

Brand fonts: Figtree · Noto Serif · Open Sans (GMU brand stack).

## Quick links

- Repo: <https://github.com/jmotis/game-sites>
- Live preview (until production domains are configured):
  - Landing: <https://jmotis.github.io/game-sites/>
  - Hub: <https://jmotis.github.io/game-sites/hub/>
  - Plague: <https://jmotis.github.io/game-sites/plague/>
  - Shipping (Learning the Ropes): <https://jmotis.github.io/game-sites/shipping/>
- Plague Twine game (external): <https://jmotis.github.io/GamingtheGreatPlague>
- Adventures in Illuminating Twine game (external): <https://jmotis.github.io/Adventures-in-Illuminating.html>

---

## Local development

### 1. Install Hugo (extended edition)

| Platform | Command |
|---|---|
| macOS | `brew install hugo` |
| Windows | `winget install Hugo.Hugo.Extended` |
| Linux | Download `.deb` from [Hugo releases](https://github.com/gohugoio/hugo/releases) and `sudo dpkg -i hugo_extended_*.deb` |

Verify: `hugo version` should print `v0.100+ … extended`.

### 2. Run a site

```bash
cd games-hub && hugo server           # → http://localhost:1313
# or, in another terminal:
cd plague-site && hugo server -p 1314 # → http://localhost:1314
```

`hugo server` watches files and live-reloads.

### 3. Build for production

```bash
cd games-hub && hugo
cd plague-site && hugo
```

Output goes to each site's `public/` (gitignored). The GitHub Actions workflow does this for you on every push to `main`.

---

## Project structure

```
.
├── themes/rrchnm/                  Shared theme (used by both sites)
│   ├── theme.toml
│   ├── layouts/
│   │   ├── _default/
│   │   │   ├── baseof.html         HTML shell (head/header/main/footer)
│   │   │   └── single.html         Generic fallback for content/*.md
│   │   ├── about/
│   │   │   └── list.html           About page (used by both sites)
│   │   └── partials/
│   │       ├── head.html           <head>: meta, fonts, CSS, theme color vars
│   │       ├── header.html         Top nav (brand mark + menu)
│   │       ├── footer.html         Red separator + copyright + Contact Us
│   │       ├── rrchnm-mark.html    SVG: 4-square C/H/N/M logo
│   │       └── orn-rule.html       Decorative section divider
│   └── static/css/
│       └── style.css               Full design system (CSS custom properties)
│
├── games-hub/                      games.rrchnm.org
│   ├── config.toml                 Site name, primary color, menu, contactEmail
│   ├── content/                    Page bodies (Markdown)
│   │   ├── _index.md               Home page intro paragraph (drop cap)
│   │   ├── about/_index.md         About page title + intro
│   │   ├── bibliography/_index.md
│   │   └── games/_index.md         Current Games page title + intro
│   ├── data/                       Structured content (YAML)
│   │   ├── games.yaml              All current games (featured: true marks home page hero)
│   │   ├── in_development.yaml     Upcoming games (smaller cards)
│   │   ├── bibliography.yaml       Bibliography entries
│   │   └── about_sections.yaml     Project Team / RRCHNM / GMU blocks
│   ├── layouts/                    Site-specific page layouts
│   │   ├── index.html              Home (hero, drop-cap intro, Featured Game, In Dev)
│   │   ├── games/list.html         Current Games (stack of game blocks + In Dev)
│   │   └── bibliography/list.html
│   └── static/images/              Hero, period images, game cover art
│
├── plague-site/                    1665plague.rrchnm.org
│   ├── config.toml
│   ├── content/
│   │   ├── _index.md               Home drop-cap intro
│   │   ├── about/_index.md
│   │   └── pedagogical-materials/_index.md
│   ├── data/
│   │   ├── game_details.yaml       Format/playtime/audience DL (dark section)
│   │   ├── lessons.yaml            Pedagogical-materials downloads
│   │   └── about_sections.yaml     (mirrors hub — same team/institutions)
│   ├── layouts/
│   │   ├── index.html              Home (hero, CTA bar, drop cap, pillars, dark "Teach")
│   │   └── pedagogical-materials/list.html
│   └── static/images/              lord-have-mercy (hero), runaways-fleeing (closing)
│
├── landing/                        Root index for the GitHub Pages preview
│   └── index.html                  Two-card preview at jmotis.github.io/game-sites/
│
├── .github/workflows/
│   └── deploy.yml                  Builds both sites & deploys to Pages
│
└── .gitignore                      Excludes public/, resources/, .hugo_build.lock
```

---

## Editing content

The pattern across the codebase: **structured/repeating content lives in `data/*.yaml`; page prose (intro paragraphs, titles) lives in `content/*.md` front matter or body**. Layouts read from both. Most edits are pure data — no template changes required.

### Add a new current game (hub)

1. Open `games-hub/data/games.yaml`.
2. Append a new entry. Required fields: `title`, `url`. Optional: `subtitle`, `description`, `image`, `image_alt`, `image_caption`, `cover`, `featured`, `teachers_url`.
3. If you want it to be the home page's "★ Featured Game" hero block, set `featured: true` (and clear it on the previous featured entry — only one should be true).
4. Drop a **square (1:1)** cover image into `games-hub/static/images/`. Reference it in `image: "your-file.jpg"`. Set `cover: true` to skip the sepia filter and treat it as a logo, not a historical engraving.
5. Image dimensions don't matter beyond the 1:1 ratio — the slot is 280×280 with `object-fit: contain`.

```yaml
- title:        "Game Title"
  subtitle:     "Place · Century · MS / HS / Undergrad"   # CSS uppercases on render
  description:  "One sentence — what's the game about?"
  url:          "https://example.com/play-the-game"
  teachers_url: "https://example.com/pedagogy"            # optional, adds For Teachers → button
  image:        "filename.jpg"                            # in static/images/
  image_alt:    "Description of the cover art for screen readers"
  cover:        true                                      # game logo (1:1, no sepia)
  featured:     false                                     # true → home-page hero
```

The new game appears automatically on:
- Hub home (only if `featured: true`)
- `/games/` Current Games page (always)

### Add an in-development game

`games-hub/data/in_development.yaml`. Same shape as games but smaller cards, no image, no teachers_url:

```yaml
- title:       "Game Title"
  subtitle:    "Place · Century · MS / HS / Undergrad"
  description: "One sentence pitch."
```

### Update bibliography (hub)

`games-hub/data/bibliography.yaml` is a list of **sections**, each with a `label` (rendered as an `<h2>` divider — same eyebrow treatment as the about page) and a list of `entries`. Each entry has a `text` field (HTML allowed via `safeHTML`) and an optional `url`.

```yaml
- label: "Scholarship"
  entries:
    - text: 'Author. (Year). <em>Title</em>. Publisher.'
      url:  'https://example.com/'
    - text: '...'

- label: "Primary Sources"
  entries:
    - text: '...'
```

Add or split sections by adding more top-level `- label:` blocks. Entries within a section render as a `<ul>` with red-square bullets.

### Update About sections (both sites)

The Hub and Plague Abouts share the same template and identical content. Edit **both** `games-hub/data/about_sections.yaml` and `plague-site/data/about_sections.yaml` to keep them in sync. Each section: `label` (eyebrow) + `body` (HTML allowed via `safeHTML`).

### Update pedagogical materials (plague)

`plague-site/data/lessons.yaml`. Each entry: `title`, `description`, `meta` (e.g. "PDF · 4 pp"), `url`. Currently all `url`s are `#` — wire them up when the actual PDFs exist.

### Update game details fact sheet (plague)

`plague-site/data/game_details.yaml`. Key/value rows shown in the dark "Teach with the game" section.

### Update the footer / contact email

`config.toml` for each site → `[params]` → `contactEmail`. Used in the mailto link and the visible button text.

### Update the navigation menu

`config.toml` → `[[menus.main]]` blocks. Each: `name`, `url`, `weight` (sort order). For external links (e.g. the Plague site's "Game" item points to the live Twine game), use a fully-qualified URL — Hugo's `relURL` passes them through unchanged.

### Update the home-page drop-cap intro

`content/_index.md`. The drop cap is rendered via CSS `::first-letter` — just write normal prose, the first letter becomes large automatically. **Don't** quote or otherwise prefix the text or `::first-letter` won't match.

---

## Adding a new game info site (cookbook)

Suppose you want a dedicated site for *Adventures in Illuminating* (like `plague-site/` is for the plague game), to live at e.g. `illuminating.rrchnm.org`.

**Standard subsite home-page layout.** Every game subsite home page follows the same six-section order, with images only at the top and bottom:

1. Hero (full-bleed image + dark gradient + title)
2. CTA bar (Play the Game / For Teachers buttons)
3. Drop-cap intro paragraph (from `content/_index.md`)
4. "Inside the Game" ornamental divider
5. Three pillar callouts
6. Closing period image (`.period-block` with caption bar)


1. **Create the directory and copy the structure.** Easiest is to copy `plague-site/` as a template:
   ```bash
   cp -r plague-site illuminating-site
   ```
2. **Update `illuminating-site/config.toml`:**
   - `baseURL` → production URL
   - `title`, `[params].siteName`, `[params].tagline`
   - `[params].primary` → site's accent color (and the related `primaryDark`/`primaryDeep`/`primarySoft` shades for AA contrast)
   - `[[menus.main]]` items → home, game URL, pedagogical-materials, about
3. **Replace content & data.** Edit each file under `content/` and `data/`. Most copy carries over (about sections especially); update home prose, game details, lessons.
4. **Drop in the new site's two images** at `illuminating-site/static/images/`. The standard subsite home page uses exactly two images: a full-bleed **hero** at the top and a **closing period image** at the bottom (rendered as a `.period-block` with caption bar). Edit `layouts/index.html` to point at your filenames, or rename your files to match the existing layout. There are no mid-page images on the home page.
5. **Hook it into the build & landing page:**
   - In `.github/workflows/deploy.yml`, add a third "Build" step modeled on the existing two:
     ```yaml
     - name: Build illuminating-site → public/illuminating/
       env:
         HUGO_ENVIRONMENT: production
       run: |
         hugo --gc --minify \
           --source illuminating-site \
           --destination "${GITHUB_WORKSPACE}/public/illuminating" \
           --baseURL "${{ steps.pages.outputs.base_url }}/illuminating/"
     ```
   - In `landing/index.html`, add a third card linking to `illuminating/` (mirror the existing two).
6. **Add to the hub.** In `games-hub/data/games.yaml`, set the Adventures entry's `teachers_url` to the new site's `/pedagogical-materials/` URL.

That's it — push and the new site appears at `https://jmotis.github.io/game-sites/illuminating/`. When the production domain is configured, the workflow's `--baseURL` override is what matters, not the value baked into the site's `config.toml`.

---

## Theme architecture

### One theme, many sites — colors via custom properties

`themes/rrchnm/static/css/style.css` defines a complete design system using CSS custom properties:

```css
:root {
  --primary: #C32A26;     /* defaults — overridden per site */
  --primary-dark: …;
  --primary-soft: …;
  --sans:  'Figtree', …;
  --serif: 'Noto Serif', …;
  --body:  'Open Sans', …;
}
```

Per-site overrides come from `themes/rrchnm/layouts/partials/head.html`, which injects an inline `<style>` block reading `.Site.Params.primary` etc. from each site's `config.toml`. **The whole site re-themes by changing four hex codes** in the config.

### URL handling — Hugo's `relURL` quirks

Both the production deploy (root domain) and the Pages preview (subpath like `/game-sites/hub/`) need to work. Two non-obvious rules:

1. **Hugo's `relURL "/about/"` returns `/about/` unchanged** (it treats leading-slash strings as already-absolute paths). To get subpath-aware URLs, drop the leading slash: `relURL "about/"` correctly returns `/game-sites/hub/about/` under a subpath baseURL.
2. **The brand link's home URL** uses `.Site.Home.RelPermalink` (not `relURL "/"`) for the same reason.
3. **Menu items configured with `url = "/about/"`** *do* get processed correctly by Hugo's menu system — those leading slashes are fine. The menu-only quirk: pipe `.URL` through `relURL` in the template (`{{ .URL | relURL }}`).

If you need to hardcode a path in a layout, use `{{ "path/" | relURL }}` (no leading slash). External URLs with a scheme (`https://…`) pass through `relURL` unchanged.

### Cover art convention

Two image styles, distinguished by the `--cover` CSS modifier:

- **`.hist-img`** (default) — historical engravings. Sepia tinted (`filter: sepia(0.18) contrast(1.05)`), `object-fit: cover` (will crop). Used for hero images, period plates, banners.
- **`.hist-img--cover`** — modern game cover art. No sepia, square 280×280 slot, centered in its column, `object-fit: contain`. Triggered by `cover: true` in `data/games.yaml`.

Game cover art should be **1:1 square** for clean rendering in the 280×280 slot. Non-square images will letterbox.

### Optional data fields

Layouts use `{{ with .field }}` to gracefully skip rendering when a field is empty or missing. This is why a game can be added with only `title` + `url` and still render correctly. Pattern:

```html
{{- with $game.subtitle }}
<p class="featured-game__meta">{{ . }}</p>
{{- end }}
```

When you add a new optional field to YAML, follow the same pattern in the template.

### Page-level layout overrides

Hugo searches each site's `layouts/` directory before falling back to the theme's. The theme provides a generic `_default/single.html` and an About layout; each site overrides home, list pages, and any page that needs custom design. To override an existing layout for one site only, copy the file from `themes/rrchnm/layouts/…` to `<site>/layouts/…` and edit.

---

## Design system reference

### Brand palette (source of truth)

The official brand palette these sites draw from. These are the canonical hex values; the per-site `--primary*` tokens below are derived from them.

| Name | Hex |
|---|---|
| George Mason Green | `#005239` |
| George Mason Gold | `#FFC733` |
| PANTONE Black 7C (Black) | `#333333` |
| PANTONE 173C (Red) | `#CC4824` |
| PANTONE 321C (Teal) | `#008285` |
| PANTONE 653C (Navy) | `#004F71` |
| PANTONE Cool Grey 10 | `#727579` |
| True Black | `#000000` |
| RRCHNM Red | `#C32A26` |

Don't substitute approximations — the brand wordmark, primary buttons, and per-site accents must use these exact values. Variants used purely for accessibility (e.g. on-dark on-light shades that meet WCAG 2.1 AA contrast) live alongside as `--primary-dark` / `--primary-bright` / etc., but the unmodified brand color must remain visible in marquee positions (header wordmark, primary CTA fill).

### Colors

| Token | Hub (RRCHNM Red) | Plague (GMU Teal) |
|---|---|---|
| `--primary` | `#C32A26` | `#008285` |
| `--primary-dark` (AA on white) | `#A02220` | `#006568` |
| `--primary-deep` | `#761815` | `#00484A` |
| `--primary-soft` (subtle bg) | `#F7E7E6` | `#E5F1F1` |
| `--ink` | `#1A1A1A` (shared) | |
| `--deep` (GMU Navy, accent) | `#004F71` (shared) | |
| `--page-bg` | `#FFFDF8` (shared) | |
| `--dark-bg` | `#0F0E0C` (shared) | |

WCAG 2.1 AA was a hard requirement. The dark/light pairings have been hand-checked but should be re-verified with axe DevTools or Lighthouse before launch (focus states on links are the most likely audit finding).

### Fonts

| Token | Stack | Used for |
|---|---|---|
| `--serif` | Noto Serif, Cambria, Georgia | Headlines, page titles, drop caps, brand wordmark |
| `--sans` | Figtree, Open Sans, Franklin Gothic, system-ui | Eyebrow labels, nav, buttons, meta |
| `--body` | Open Sans, Figtree, Franklin Gothic, system-ui | Reading copy, body paragraphs |

Loaded from Google Fonts via `themes/rrchnm/layouts/partials/head.html`. Fallbacks are GMU's "approved alternates" (Cambria, Franklin Gothic).

### Reusable components / class names

- `.site-header` / `.site-nav` — top bar
- `.hero` + `.hero__img` + `.hero__title` — full-bleed hero with gradient overlay
- `.cta-bar` (plague only) — light bar below hero with two centered buttons
- `.btn` + `.btn--primary` / `.btn--inverse` / `.btn--ghost` / `.btn--contact` / `.btn--download`
- `.btn-row` — flex container for two adjacent buttons (e.g. Play the Game + For Teachers)
- `.featured-game` + `.featured-game__grid` — red full-width block, used on home Featured + every entry on Current Games
- `.featured-game__grid--solo` — single-column variant when a game has no image yet
- `.card` + `.card__meta` + `.card__title` + `.card__desc` — In Development mini-cards
- `.orn-rule` — decorative section divider (line + ornament + label + ornament + line)
- `.pillars` + `.pillar` (plague) — three-column inside-the-game block
- `.dark-section` (plague) — dark "Teach with the game" panel with DL on right
- `.bib-list` + `.bib-entry` — bibliography list with red-square bullets
- `.lessons` + `.lesson` (+ `.lesson--alt`) — alternating-row pedagogy grid
- `.about-section` + `.about-section__label` + `.about-section__body` — about page sections
- `.hist-img` (sepia) / `.hist-img--cover` (no sepia, square)
- `.drop-cap-section` (+ `--plague` for larger cap) — first-letter drop cap via CSS `::first-letter`

---

## Deployment

### GitHub Pages (current)

Pushes to `main` trigger `.github/workflows/deploy.yml`. The workflow:

1. Installs Hugo extended.
2. Builds `games-hub/` with `--baseURL "<pages-base>/hub/"` → `public/hub/`.
3. Builds `plague-site/` with `--baseURL "<pages-base>/plague/"` → `public/plague/`.
4. Copies `landing/index.html` to `public/index.html`.
5. Uploads & deploys the combined `public/` to GitHub Pages.

**One-time setup** (already done): repo Settings → Pages → Build and deployment → Source: **GitHub Actions** (not "Deploy from a branch").

### Production (custom domains)

When `games.rrchnm.org` and `1665plague.rrchnm.org` are ready:

1. Each site needs its own deploy (separate hosting, separate repo, or a different workflow that targets each domain).
2. The `baseURL` in each site's `config.toml` is already correct for production. The Pages workflow's `--baseURL` flag overrides it for the preview.
3. **Update the `teachers_url` in `games-hub/data/games.yaml`** from the github.io subpath to the production URL `https://1665plague.rrchnm.org/pedagogical-materials/`. There's a `TODO` comment marking the line.
4. Production won't need the subpath URL gymnastics in the templates — the existing path-handling works correctly at root.

---

## Open TODOs

Search the codebase for `TODO` to find the live list. Currently:

- `games-hub/data/games.yaml` — Plague `teachers_url` to swap to production at deploy
- `games-hub/data/games.yaml` — Adventures in Illuminating still needs real `subtitle` (it has one), `image_alt` (has one), and a `teachers_url` if pedagogy materials exist
- `games-hub/data/in_development.yaml` — Learning the Ropes has its subtitle/description; nothing pending
- `plague-site/data/lessons.yaml` — every `url` is `#` placeholder; wire up real PDF URLs
- `games-hub/data/about_sections.yaml` & `plague-site/data/about_sections.yaml` — RRCHNM blurb has a probable typo: *"the center which as developed"* → likely *"which has developed"*

### Things never built (in the original design but unimplemented)

- Plague About page styling beyond the inherited About template (currently uses generic title "About"; consider a stylistically-parallel title like "Behind the game.")
- Skip-link styling beyond the basic visible-on-focus; visible focus states on nav links could be more pronounced
- Mobile/responsive review — the layout is desktop-first; narrow viewports haven't been audited
- Real 404 page (Hugo defaults to a basic one)

---

## Working in a new Claude session

If you want to continue this work in a new chat, here's what an AI assistant needs to know up front:

1. **Source of design intent.** The original prototypes from Claude Design (HTML/JSX in a separate handoff bundle) are *not* in this repo. They've been faithfully translated into the Hugo theme; the theme's CSS is now the source of truth for the design. Direction D was the chosen direction (Broadside-style body + editorial header).

2. **Two sites, one theme.** Don't accidentally duplicate logic across `games-hub/` and `plague-site/` — most things belong in `themes/rrchnm/`. Site-level overrides exist only for layouts that genuinely differ (home pages, the games list, etc.).

3. **Data-driven content.** Adding a game, lesson, bibliography entry, or about section is a YAML edit, not a template edit. Resist the urge to hardcode content into layouts.

4. **Hugo gotchas already encountered.**
   - `relURL "/path/"` doesn't add subpath — drop the leading slash.
   - `.Site.Home.RelPermalink` for the brand-link home URL.
   - `with` shadows the context — capture the loop variable (`{{ $game := . }}`) before entering a `with`/`if` block if you need its sibling fields.
   - Commit signing fails in this environment — use `git -c commit.gpgsign=false commit …` (or set `commit.gpgsign=false` locally).

5. **The site builds clean** under both root baseURL (production) and subpath baseURL (Pages preview). Test under both before pushing template changes:
   ```bash
   hugo -s games-hub --destination /tmp/test-root
   hugo -s games-hub --baseURL "https://jmotis.github.io/game-sites/hub/" --destination /tmp/test-sub
   ```

6. **Audience.** High-school and college history teachers (and their students). WCAG 2.1 AA is a hard requirement. Tone is academic-editorial, not playful.

7. **Asset conventions.**
   - Game cover art: 1:1 square, modern logo style (no sepia applied).
   - Historical engravings: any aspect ratio, sepia-tinted, used for heroes/banners.
   - Images live in each site's `static/images/` (not in the theme).

8. **Deploy = push to main.** GitHub Actions handles the rest. Don't commit `public/` (gitignored).

9. **Tokens / secrets.** When pushing on the user's behalf, use a personal access token *they* provide. Don't store it; scrub it from the git remote URL after the push. Tokens shared in chat should be revoked after use.
