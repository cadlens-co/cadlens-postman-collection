# Changelog

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
