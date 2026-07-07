# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A bilingual (French default / English) Jekyll site for Dynamium, a robotics research lab at
Université de Moncton, deployed to GitHub Pages. No build tooling beyond Jekyll itself — no
package.json, no bundler config committed (see "Running locally" below to set one up ad hoc).

## Commands

There's no committed Gemfile. To build or preview locally:

```
bundle init
bundle add jekyll
bundle exec jekyll serve
```

Then open http://localhost:4000 (French) and http://localhost:4000/en/ (English).

CI (`.github/workflows/jekyll.yml`) builds the site on every push/PR to `main` using the
`jekyll/builder:latest` Docker image (`jekyll build --future`) — it only validates the build, it
doesn't deploy. Actual publishing happens via GitHub Pages picking up `main` directly.

There are no tests and no linter configured for this repo.

## Architecture: single-page, two-language site

The site was fully rewritten from the old multi-page `minima` Jekyll theme to a custom single-page
design (see the `overhaul` commit) — there is no theme dependency left (`_sass/minima`,
`minima.gemspec`, and the old `header.html`/`footer.html`/`home`/`page` layouts are gone). Just two
top-level pages exist:

- `index.html` (French, served at `/`)
- `en/index.html` (English, served at `/en/`)

Both use `layout: default` (`_layouts/default.html`), which has its own fully inline nav/hero/
footer markup — no `_includes` are used at all. Research, members, and news are all rendered as
anchor sections (`#research`, `#members`, `#news`, `#contact`) directly inside each `index.html`,
sourced from `_data/*.yml`. **`index.html` and `en/index.html` are near-duplicates of each other**
— the only differences are front matter (`lang`, `permalink`) and the data-field suffix used
(`_fr`/`_en`); a content/structure change to one almost always needs the same change in the other.

## Bilingual data pattern

Both languages are driven by the same `_data/*.yml` files, using a field-suffix convention rather
than separate files per language:

- `_data/members.yml` — one entry per person: `role_en`/`role_fr`, `section` (must be one of
  `professors`, `staff_grad`, `undergrad`, `alumni` — order controlled by `members_sections` in
  `_config.yml`), optional `tags_en`/`tags_fr`, optional `photo` (omit for an initials
  placeholder), optional `photo_position`, optional `link`.
- `_data/research.yml` — `title_en`/`title_fr`, `description_en`/`description_fr` per research
  area card.
- `_data/strings.yml` — all UI copy (nav labels, hero text, section headings, footer), keyed by
  `en:`/`fr:` top-level blocks, accessed in templates as `site.data.strings[page.lang].xxx`.

Templates pick the right field dynamically via Liquid, e.g. `area["title_" | append: page.lang]`
— so adding a new bilingual field means adding both `_en`/`_fr` keys to the data file, no template
branching needed as long as the existing `["field_" | append: page.lang]` pattern is followed.

News posts follow the same "one file per language per story" convention instead: each post in
`_posts/` has a `lang: en` or `lang: fr` front-matter field and its own permalink (e.g.
`2025-09-15-icarm-2025.md` for English, `2025-09-15-fr-icarm-2025.md` for French); each homepage
variant filters `site.posts` by its own `page.lang` and shows the 6 most recent.

## Brand / style

Colors: `#02488C` (navy, used sparingly) and `#E0A725` (gold) as accents on a near-black `#0A1620`
background. Fonts: Space Grotesk (body/headings) and IBM Plex Mono (nav, labels, mono accents),
loaded from Google Fonts in `_layouts/default.html`. Styling on the live homepage is entirely
inline `style=` attributes, not classes/SCSS — follow that convention when editing
`index.html`/`en/index.html`/`_layouts/default.html` rather than introducing a new stylesheet.
