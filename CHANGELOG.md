# Changelog

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
