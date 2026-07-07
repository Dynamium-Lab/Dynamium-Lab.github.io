# Dynamium site — Jekyll (bilingual: FR default / EN)

A Jekyll version of the Dynamium site for GitHub Pages, with French as the default language and an
English version at `/en/`.

## How it works

- **French** lives at the site root (`/`) — `index.html`.
- **English** lives at `/en/` — `en/index.html`.
- Both pages pull from the same data files, picking the right language per field
  (e.g. `role_en` / `role_fr`). The "FR"/"EN" pill in the nav switches between them.
- News posts are one file per language per story (e.g. `2025-09-15-icarm-2025.md` for English,
  `2025-09-15-fr-icarm-2025.md` for French) — each post has a `lang: en` or `lang: fr` front-matter
  field, and each page only lists posts matching its own language.

## How to update content

- **Members** — edit `_data/members.yml`. Each person needs `role_en`/`role_fr` and a `section`
  (one of `professors`, `staff_grad`, `undergrad`, `alumni` — see `_config.yml`). `photo` is optional;
  omit it to show initials instead.
- **News** — add two new files to `_posts/`: one English (`YYYY-MM-DD-slug.md`, `lang: en`) and one
  French (`YYYY-MM-DD-fr-slug.md`, `lang: fr`). Each page shows its own 6 most recent posts.
- **Research areas** — edit `_data/research.yml` (`title_en`/`title_fr`, `description_en`/`description_fr`).
- **UI labels / hero copy / section headings** — edit `_data/strings.yml` (one block per language).

## Running locally (optional, to preview before pushing)

```
bundle init
bundle add jekyll
bundle exec jekyll serve
```
Then open http://localhost:4000 (French) and http://localhost:4000/en/ (English).

## Publishing on GitHub Pages

1. Push this folder's contents to the root of your GitHub Pages repo.
2. In `_config.yml`, set `url` to your site's address and `baseurl` if the site lives in a subpath
   (leave both as-is for a custom domain or a `username.github.io` user page).
3. Commit and push — GitHub Pages rebuilds automatically within a minute or two.

## Design notes

Layout and one-off page markup (nav, footer, hero, section wrappers) live in `_layouts/default.html`,
`index.html`, and `en/index.html` — you generally shouldn't need to touch these for routine updates.
Colors: `#02488C` (navy, used sparingly) and `#E0A725` (gold) are the brand accents on a near-black
`#0A1620` background.
