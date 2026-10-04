# AGENTS.md

Personal portfolio + blog built with [Hugo](https://gohugo.io/), using the
custom theme in `themes/portfolio/` (styled after NearlyFreeSpeech.NET).
Content is generated from a local Obsidian vault.

## Commands

- `hugo server` — run the local dev server.
- `hugo build` — build the site into `public/`.
- `./build.sh` — full CI build (installs tooling, runs `hugo build --gc --minify`).
- `python3 scripts/sync_vault.py` — sync the Obsidian vault into `content/` (local only).

## Layout

- `themes/portfolio/` — theme (layouts in `layouts/`, styles in `static/css/main.css`).
- `content/` — site content (`_index.md`, `about/`, `learning/`, `courses/`, `projects/`, `posts/`).
- `hugo.toml` — site config, menus (`main` = top tabs, `sidebar`, `footer`).
- `wrangler.jsonc` + `build.sh` — Cloudflare Pages deployment.
- `scripts/sync_vault.py` — Obsidian→Hugo sync (gitignored, local-only).
- `notes/` — session notes (gitignored).

## Obsidian sync

The site mirrors an Obsidian vault. `scripts/sync_vault.py` reads the vault
and writes normalized Hugo content into `content/`:

- Vault path: `OBSIDIAN_VAULT` env var (default
  `/home/marty-manguerra/Documents/Obsidian Vault`).
- Folder mapping: `About/` → `about/` (bio → `index.md`), `Learning/` →
  `learning/`, `Courses/` → `courses/`, `Projects/` → `projects/`
  (subfolders preserved); `Posts/` → `posts/` (blog).
- Transform: adds Hugo frontmatter (`title`/`date`/`draft`/`tags`), resolves
  `[[wikilinks]]` to Hugo URLs (broken links become plain text), promotes the
  leading `# H1` to `title`.
- The script is local-only (not run in CI); run it and commit the result.

## Conventions

- Accent/brand color is green `#228427`; visited links `#114413`.
- Nav active state is derived by URL matching in `layouts/partials/nav.html`
  and `sidebar.html`, not `IsMenuCurrent`.
