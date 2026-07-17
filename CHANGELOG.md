# Changelog

## API service — 2026-07-17 (no SDK changes required)

- Parser 2.2.1: drawings with ACIS 3DSOLID geometry no longer drop circular
  edges (mounting-plate holes, cable-gland openings, bolt circles) from the
  extracted wireframe — a record-layout mis-alignment skipped exactly those
  circles and could inflate drawing extents with mis-read arcs. All solid
  edges are now extracted; results report `parserVersion: "2.2.1"`. No
  request/response shape changes; 2D drawings are unaffected.

## API service — 2026-07-16 (no SDK changes required)

- Large-file parses are ~3x faster: redundant conversion retries that could
  not change the outcome are skipped and the streaming geometry filter
  fast-forwards over out-of-budget sections. Results are identical (same
  entities, layers, `truncated` flag). Conversion time limits for very
  complex drawings are also higher and configurable server-side. No
  request/response shape changes.

## API service — 2026-07-15 (no SDK changes required)

- `mode=sync` uploads now auto-divert to async when the file is large or its
  converted geometry turns out huge: the API returns `202` immediately with a
  human-readable `message` instead of holding the connection until the sync
  wait times out. Poll `GET /v1/jobs/:jobId` or use webhooks as usual.
- Oversized drawings that previously failed with `FILE_TOO_COMPLEX` are now
  parsed with a bounded streaming pre-filter and return `truncated: true` in
  the result summary. No request/response shape changes.

## API service — 2026-07-14 (no SDK changes required)

- Large-file reliability fix on the API: drawings whose converted geometry
  exceeds processing limits now fail fast with status `FAILED` and a clear
  error message (previously they could remain `PROCESSING` indefinitely after
  a server interruption). Failure emails and `job.failed` webhooks now fire
  for every terminal outcome. No request/response shape changes.

## [0.6.0] — 2026-07-13

### Changed (API Schema v2.0.0 — BREAKING for entity consumers)
- Added a saved 200 example to `GET /v1/jobs/:jobId/result` showing the Schema
  v2 envelope: top-level `schemaVersion`/`parserVersion`/`parseInfo`,
  `summary.statistics` (`byType`/`byCategory`), `sheets[].key`, and entities
  with `geometry`-grouped coordinates plus `handle`, `category`, `text`,
  `reference`, `properties`, and always-present `bbox`/`metrics` siblings.
  Flat entity coordinate fields (`start`, `end`, `vertices`, …) no longer
  appear at the entity root — read them from `entity.geometry`.

## [0.5.0] — 2026-07-07

### Added
- `GET /v1/jobs/:job_id/result` example: `metadata.unsupported3DCount` — count
  of 3D-only entity types (3DSOLID/BODY/SURFACE/REGION/MESH) with no
  extractable geometry.

### Fixed (API behaviour, no schema change)
- `metadata.is3D` no longer reports `true` for drawings whose only 3D content
  is unsupported entity types with an empty 3D scene.

## [0.4.0] — 2026-07-06

### Notes (API v1.4.1)
- API responses are now gzip-compressed via standard content negotiation
  (`Accept-Encoding`). HTTP clients handle this automatically — no client
  changes required; large results simply download ~12× faster.

### Added
- `POST /v1/parse`: optional `notifyEmail` form field (disabled by default) —
  CADLens emails a job link when the parse finishes and the uploader stopped watching.
- `GET /v1/jobs/:jobId`: optional `watch=1` query param (disabled by default) for
  interactive poll loops; suppresses the notify email when the user watches the
  job finish live.

### Changed
- API v1.4.0: `job.completed` webhook payloads now carry per-sheet metadata only
  (no `entities`/`layers`) plus a `resultUrl` pointing at `GET /v1/jobs/:id/result`
  for full geometry. Payloads over 256 KB omit `sheets` entirely.

## [0.3.0] — 2026-07-02

### Updated
- `GET /v1/jobs/:job_id/result` example: added a `HATCH` entity showing the new
  `patternLines` field (exact hatch pattern geometry) and reliably-populated
  `patternAngle` / `patternScale`; added `metadata.linetypePatterns` and
  `metadata.ltscale` to the response shape.
- Bumped collection `info.version` to `0.3.0`.

## [0.2.0] — 2026-06-25

### Updated
- Added `"key"` field to sheet object in `GET /v1/jobs/:job_id/result` response example.
  `key` is a unique HTML-safe slug for each sheet (e.g. `"floor-plan-2"`).
- Bumped collection `info.version` to `0.2.0`.

## [0.1.0] — 2026-06-19

Initial collection with sheets-based response schema.
