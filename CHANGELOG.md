# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## Unreleased

### Fixed
- Fix `get-all-activities` filtering by `activityTypes` by preserving `type` and `sport_type` fields from the Strava `athlete/activities` response schema.
- Handle Strava's 64-bit Activity and Athlete identifiers (effective June 1, 2027). IDs are now read from `id_str` and shown exactly instead of as a rounded JavaScript number, and tools that take an activity or athlete ID accept it as a string. A numeric ID beyond the safe integer range is rejected with a message asking for a string, rather than silently querying the wrong object.
- Show route IDs from `id_str` in `list-athlete-routes` and `get-route` so they can be passed back exactly.

### Changed
- `disconnect-strava` now revokes the token on Strava through the `oauth/revoke` endpoint before removing stored credentials (`oauth/deauthorize` is retired on June 1, 2027). Local credentials are removed even if the revoke call fails; pass `revoke: false` to skip it.

### Added
- Add `STRAVA_API_BASE_URL` to override the Strava API base URL, so the move to `https://www.api-v3.strava.com` (available January 4, 2027; required by June 1, 2027) can be made without a code change. The default is unchanged for now because the new host is not live yet.
- Add Vitest test runner and regression tests for `get-all-activities` filtering and schema preservation.
- Add a stdio-based MCP smoke test script to validate tool behavior against the built `dist/server.js`.
- Add `perceived_exertion` data in activity detailed model.
