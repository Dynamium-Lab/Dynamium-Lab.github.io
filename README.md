# Dynamium website

Source of https://www.dynamium.ca, the website of Dynamium, the Dynamic Systems Laboratory at Université de Moncton. Built with Jekyll and published by GitHub Pages from `main`. French is the default language (`/`); English is at `/en/`.

## Updating content

- **Members:** edit `_data/members.yml`. Each person needs `role_en`, `role_fr` and a `section` (`professors`, `staff_grad`, `undergrad` or `alumni`). `photo` is optional; without it the card shows initials.
- **News:** add two files to `_posts/`, one English (`YYYY-MM-DD-slug.md`, `lang: en`) and one French (`YYYY-MM-DD-fr-slug.md`, `lang: fr`), each with its own `permalink`. The homepage shows the 6 most recent posts in each language.
- **Research areas:** edit `_data/research.yml`.
- **Interface text, hero, address:** edit `_data/strings.yml` (one block per language).
- **Page structure:** `_includes/home.html` (homepage, both languages) and `_layouts/default.html` (header, footer, styles).

## Previewing locally

```
bundle init
bundle add jekyll jekyll-feed
bundle exec jekyll serve
```

Then open http://localhost:4000 (French) and http://localhost:4000/en/ (English).

On Windows without Ruby, the same works inside WSL (Ubuntu):

```
sudo apt install -y ruby-full build-essential
gem install --user-install bundler jekyll jekyll-feed
cd /mnt/c/work/code/dynamium-site
~/.local/share/gem/ruby/*/bin/jekyll serve
```

GitHub Pages has no per-branch preview: the live site changes only when a pull request is merged into `main`.

## Publishing

Merge into `main`. GitHub Pages rebuilds the site within a minute or two. The CI workflow builds every pull request to catch Liquid or YAML errors before merging.
