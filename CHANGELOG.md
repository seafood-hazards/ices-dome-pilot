# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
As this project is still in active development, it does not yet strictly adhere to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.19] - 2026-08-07
### Added
- Pipeline Generations section on the home page (`_generations.qmd`), with links to the other four pilot sites and to the slim, clean, merged and refined generation sites

### Changed
- Database file renamed from `pilot_ices_dome.sqlite` to `ices_dome_pilot.sqlite`, matching the engine's `<source>_pilot.sqlite` convention
- Database is downloaded from the latest GitHub release instead of a pinned release tag, so no version string has to be edited when a new database is published
- Database Downloads page lists the single pilot database and links to the latest release
- Grain Size Codes moved to the DB Design menu, and the Data Export menu renamed to EFSA Submission
- CLAUDE.md reduced to the site's invariants, with the detail moved to `docs/database.md` and `docs/site.md`

### Removed
- Export to Tabular File page: the pilot generation no longer exports a dataset file
- DB Schema (Slim) page and the slim database download, which belong to the slim generation's own site
- Unreferenced `.data/` files (`ices_dome_projects.tsv.gz`, `points_web_ices_dome.Rds`), which no page reads

## [0.1.18] - 2026-07-24
### Added
- Database Downloads page linking to the full and slim SQLite database files

### Changed
- bumped database release references to v0.1.17 (`download_resources.R`, `_db-setup.qmd`, `data-export.qmd`)

## [0.1.17] - 2026-07-21
### Changed
- moved `matrix` column from subsample to measurement table in the slim schema
- updated slim schema diagram to reflect the `matrix` column move

## [0.1.16] - 2026-07-17
### Fixed
- slim DB schema page: lat/long rounding description now matches stated precision (3 d.p.)
- CHANGELOG.md: missing bullet marker and Vannmiljø spelling

## [0.1.15] - 2026-07-14
### Changed
- renamed "DB Schema" nav entry to "DB Schema (Full)" to distinguish it from "DB Schema (Slim)"

## [0.1.14] - 2026-07-14
### Added
- slim DB schema page (`db-schema-slim.qmd`) documenting a common, multi-source schema

## [0.1.13] - 2026-07-13
### Added
- CLAUDE.md project guidance and documented deployment/branching in README
- EFSA format and submission pages (v1 and v2) under Data Export

### Fixed
- typos in EFSA format/submission pages
- renv.lock out of sync (dplyr and its dependencies not recorded)

## [0.1.12] - 2026-05-07
### Fixed
- average calculation for the interactive map

## [0.1.11] - 2026-04-27
### Changed
- index page for new pages

## [0.1.10] - 2026-04-27
### Fixed
- sediment numbers

## [0.1.9] - 2026-04-26
### Removed
- image files of distances to coast

### Changed
- all renv entries

### Fixed
- table column format

## [0.1.8] - 2026-04-24
### Fixed
- selection box in code lookup browser page

## [0.1.7] - 2026-04-24
### Added
- code lookup browser page
- grain size parameter page

## [0.1.6] - 2026-04-18
### Changed
- latitude and longitude order
- data sources from data file to db

### Added
- link to GitHub on menu

## [0.1.5] - 2026-04-16
### Added
- country list to sediment map

## [0.1.4] - 2026-04-16
### Changed
- data source for interactive site map

## [0.1.3] - 2026-04-15
### Fixed
- Interactive sediment map

### Added
- DOI link to Zenodo in README

## [0.1.2] - 2026-04-15
### Added
- Interactive sediment map

## [0.1.1] - 2026-04-14
### Added
- Update all pages to replace Vannmiljø data

## [0.1.0] - 2026-04-13
### Added
- Initial Quarto pages
