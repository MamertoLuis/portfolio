# AGENTS.md

Personal portfolio + blog built with [Hugo](https://gohugo.io/), using the
custom theme in `themes/portfolio/` (styled after NearlyFreeSpeech.NET).

## Commands

- `hugo server` — run the local dev server.
- `hugo build` — build the site into `public/`.
- `./build.sh` — full CI build (installs tooling, runs `hugo build --gc --minify`).
- `hugo new blog/<name>.md` — scaffold a new post (set `draft = false` to publish).

## Layout

- `themes/portfolio/` — theme (layouts in `layouts/`, styles in `static/css/main.css`).
- `content/` — site content (`_index.md`, `about/`, `blog/`).
- `hugo.toml` — site config, menus (`main` = top tabs, `sidebar`, `footer`).
- `wrangler.jsonc` + `build.sh` — Cloudflare Pages deployment.
- `notes/` — session notes (gitignored).

## Conventions

- Accent/brand color is green `#228427`; visited links `#114413`.
- Nav active state is derived by URL matching in `layouts/partials/nav.html`
  and `sidebar.html`, not `IsMenuCurrent`.
