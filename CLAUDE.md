# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Quarto website (source `.qmd` files in repo root) → static site deployed to GitHub Pages. No app server, no build framework beyond Quarto/R. Presents a pilot relational DB design built from public ICES-DOME sediment contaminant data.

## Commands

- Render site locally: RStudio "Render Website", or `quarto render`
- Restore R package env (required before first render): `renv::restore()`
- No test suite, no linter configured.

## Architecture

- **Data**: `pilot_ices_dome.sqlite` (gitignored, ~35MB) is NOT checked in. `_quarto.yml` sets `pre-render: download_resources.R`, which fetches the sqlite file + sql.js/stratum-sqlite WASM libs from GitHub Releases into `libs/sqljs/` before every render. Don't hand-edit files under `libs/`.
- **No backend**: every `.qmd` page queries the SQLite file *in the browser* via sql.js/stratum-sqlite (WASM), not server-side R. Query logic lives in Observable JS `{ojs}` cells, not `{r}` cells (R cells are mostly used for static reference tables, e.g. `tribble()` in `db-schema.qmd`).
- **Shared DB connection**: every page that queries the DB starts with `{{< include _db-setup.qmd >}}`, which opens the shared `db` object. Any new data-driven page needs this include.
- **`header.html`**: injected site-wide via `_quarto.yml`'s `include-in-header`. Sets up `window._sqljsBase` / `window._dbPath` dynamically based on page depth (so pages work at any subdirectory nesting) — don't hardcode asset paths in pages.
- **Nav/page registration**: adding a page requires both creating the `.qmd` file AND adding it to the `navbar` in `_quarto.yml`.
- **DB schema**: 10-table normalized schema (`project`, `site`, `sample`, `parameter`, `sediment` fact table, `lld`, `analysis_method`, `reference`, `code_lookup`, + geospatial fields on `site`). Full column definitions are documented in `db-schema.qmd` — check there before writing new queries. A second, generalized 7-table "slim" schema (shareable across multiple source projects) is documented in `db-schema-slim.qmd`.
- **Database downloads**: `database-downloads.qmd` links to the prebuilt SQLite files (full and slim schema) attached as assets on the same GitHub release. The release tag is hardcoded in three places that must stay in sync when a new DB version is published: `download_resources.R` (`db_url`), `_db-setup.qmd` (`cacheKey`), and `data-export.qmd` (tsv.gz download link) — plus the links in `database-downloads.qmd` itself.
- **CI/CD**: `.github/workflows/publish.yml` — push to `main` runs `quarto render` (via renv-restored R env) and deploys `_site/` to GitHub Pages. No separate build config; local and CI render the same way.
- **Changelog**: `CHANGELOG.md` follows Keep a Changelog format. Update `[Unreleased]` when making notable changes.

## Git workflow

- Gitflow: `main` (releases), `develop` (integration), `feature/*`, `release/*`.
- **No PRs** — feature branches are merged directly into `develop` (and release branches into `main`/`develop`) with a merge commit, then deleted. Do not propose opening a GitHub PR unless explicitly asked.
