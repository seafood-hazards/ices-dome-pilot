# Database

`ices_dome_pilot.sqlite` is the **only** file this site depends on. It is built
by the `multised-engine` package (`create_db("pilot", "ices-dome")`), not in this
repository, and is published as an asset on this repository's latest GitHub
release.

The name changed from `pilot_ices_dome.sqlite` at site v0.1.19, matching the
engine's `<source>_pilot.sqlite` convention.

## Schema

Ten tables, the largest of the five pilot databases. All access is client-side,
through stratum-sqlite.

| Table | PK | Rows | Notes |
|---|---|--:|---|
| `sediment` | `project_id, site_id, sample_id, param, sediment_no` | 345,809 | the fact table: value, unit, basis, matrix, qflag, vflag, uncrt, metcu, depth_from, depth_to, sub_no, dcflag |
| `sample` | `sample_id` | 17,694 | year, date, sample_type, sample_type_description |
| `site` | `site_id` | 11,306 | station, latitude, longitude, dist_to_coast, est_country, country_code, municipality, sea_name |
| `analysis_method` | `analysis_id` | 2,609 | param, labo, and the metst/metpt/metps/metcx/metoa method codes |
| `lld` | `lld_id` | 1,536 | lod and loq per parameter |
| `code_lookup` | `data_col, code_type, raw_code, code` | 708 | the ICES code vocabularies, resolved to descriptions |
| `project` | `project_id` | 148 | project, purpose, country, institute |
| `parameter` | `param` | 142 | param_description, group_code, group_description |
| `reference` | `ref_id` | 27 | ref, ref_description |

Site coordinates are rounded to 3 decimal places. The nearest-country column is
`est_country` here, not `country`.

## The geo columns

`site.dist_to_coast`, `est_country`, `country_code`, `municipality` and
`sea_name` are computed by the external
[seastamp](https://github.com/AIQC-Hub/seastamp) CLI (GSHHG full resolution,
Natural Earth 1:10m, GISCO LAU 2021, IHO Sea Areas v3), run over the distinct
site positions in an LAEA projection derived from the points themselves. They
are **not** in the raw ICES-DOME export.

They were recomputed at site v0.1.20: before that they came from an `sf` /
`rnaturalearth` / `giscoR` implementation, which resolved `sea_name` to only
four ocean basins across the whole of European waters. `distance-to-coast.qmd`
and `location-names.qmd` document the method and the measured change.

`est_country` is the country a site is *nearest to*, not the country that
reported it: the reporting institute is on `project`.

`code_lookup` is what makes this source verbose: ICES reports most fields as
codes, and the table resolves them by `code_type`. `code-lookup-browser.qmd`
browses it, and `sediment-fractions.qmd` uses the `MATRX` codes to document the
grain size vocabulary. That vocabulary is large, which is why ICES-DOME
contributes the richest sediment composition data to the later generations.

`db-schema.qmd` renders the ER diagram and the full column definitions;
`data-preparation.qmd` documents how the raw ICES-DOME export was transformed
before import.

## Querying it from a page

Every page that reads the database includes `_db-setup.qmd`:

```qmd
{{< include _db-setup.qmd >}}
```

`header.html` sets `window._stratumSQLite`, `window._dbPath` and
`window._sqljsBase` at page load; `_db-setup.qmd` opens the file and exposes it
as `db`, which is then available to every OJS block on that page. Query logic
belongs in `{ojs}` cells; `{r}` cells are for static reference tables.

**One database per page.** Opening a second one on the same page fails.

## The cache key

stratum-sqlite caches the database in the browser, keyed by the `cacheKey` in
`_db-setup.qmd` (`ices-dome-pilot@vX.Y.Z`). Whenever the database **content**
changes, bump that key, or returning browsers keep serving the stale cached copy
and queries fail with "no such column".

This is the one version string that still has to be edited by hand; the download
URLs resolve to the latest release on their own.

## Scope

This site documents the **pilot** generation only. The slim, clean, merged and
refined generations have their own sites, and nothing here should link to their
schemas or ship their database files.
