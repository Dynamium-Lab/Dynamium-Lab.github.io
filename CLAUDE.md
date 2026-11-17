# CLAUDE.md

Guidance for Claude Code when working in this repository. This file is excluded from the Jekyll build (see `exclude:` in `_config.yml`), so it is not published.

## What this is

A bilingual (French default, English) Jekyll site for Dynamium, the Dynamic Systems Laboratory at Université de Moncton, published with GitHub Pages at https://www.dynamium.ca. No build tooling beyond Jekyll itself: no package.json, no committed Gemfile.

## Commands

To build or preview locally:

```
bundle init
bundle add jekyll jekyll-feed
bundle exec jekyll serve
```

Then open http://localhost:4000 (French) and http://localhost:4000/en/ (English). On a Windows machine without Ruby, use WSL (see `README.md`). Members with no photo show initials; alumni are listed without photos.

CI (`.github/workflows/jekyll.yml`) builds the site on every push and pull request to `main` with the `jekyll/builder` Docker image (`jekyll build --future`). It only validates the build; GitHub Pages publishes `main` directly. There are no tests and no linter.

## Structure

- `index.html` (French, `/`) and `en/index.html` (English, `/en/`) contain only front matter and `{% include home.html %}`.
- `_includes/home.html` holds the whole homepage body (hero, research, members, news, contact), shared by both languages. Change it once; both pages follow.
- `_includes/date.html` formats a date in the page language (`{% include date.html date=post.date lang=page.lang %}`), using the `months` list in `_data/strings.yml`. Do not use `date: "%b"`, which always prints English month names.
- `_layouts/default.html` holds the header, footer, meta tags (canonical, hreflang, Open Graph) and the single `<style>` block with all CSS. Colors are CSS variables on `:root`.
- `_layouts/post.html` renders one news post.

## Bilingual data pattern

Both languages are driven by the same `_data/*.yml` files, using a field-suffix convention:

- `_data/members.yml`: one entry per person, `role_en`/`role_fr`, `section` (one of `professors`, `staff_grad`, `undergrad`, `alumni`, ordered by `members_sections` in `_config.yml`), optional `tags_en`/`tags_fr`, `photo`, `photo_position`, `link`.
- `_data/research.yml`: `title_en`/`title_fr`, `description_en`/`description_fr` per research area.
- `_data/strings.yml`: all UI copy, lab and address details, keyed by `en:`/`fr:` blocks, accessed as `site.data.strings[page.lang].xxx`.

Templates select the field with `area["title_" | append: page.lang]`, so a new bilingual field only needs both `_en`/`_fr` keys.

News posts are one file per language per story (`2025-09-15-icarm-2025.md` and `2025-09-15-fr-icarm-2025.md`), each with `lang: en` or `lang: fr` and its own permalink. Each homepage lists the 6 most recent posts in its language. An optional `image:` in front matter sets the Open Graph preview image.

## Style

Colors: near-black navy background `#0A1620`, gold accent `#E0A725` used sparingly (buttons, links). The logo is light blue and gold on a transparent background, so the site stays on a dark background. Font: Source Sans 3 from Google Fonts.

Writing: plain, factual academic tone. No em dashes, no arrows in buttons, no all-caps labels, no emoji, no marketing superlatives ("cutting-edge", "pushing the boundaries"). News posts state who, what, where, and the funding source.
