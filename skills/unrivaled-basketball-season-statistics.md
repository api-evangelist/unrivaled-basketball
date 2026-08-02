---
name: Pull Unrivaled season standings, leaders, and statistics
description: Resolve available seasons, then pull standings, rankings, leaders, and per-team seasonal
  statistics; use the Daily Change Log for incremental refresh.
api: openapi/unrivaled-basketball-openapi.yml
operations:
- get_{access_level}v8{language}leagueseasons.{format}
- get_{access_level}v8{language}seasons{season_year}{season_type}standings.{format}
- get_{access_level}v8{language_code}seasons{season_year}{season_type}leaders.{format}-1
- get_{access_level}v8{language_code}leaguehierarchy.{format}-1
- get_{access_level}v8{language}seasons{season_year}{season_type}teams{team_id}statistics.{format}
- get_new-endpoint
generated: '2026-07-21'
method: generated
---

## Steps

1. **Resolve seasons** — call Seasons
   (`/{access_level}/v8/{language}/league/seasons.{format}`, operationId `get_{access_level}v8{language}leagueseasons.{format}`)
   to list valid `season_year` + `season_type` pairs.
2. **Standings and rankings** — call Standings
   (`/{access_level}/v8/{language}/seasons/{season_year}/{season_type}/standings.{format}`, operationId
   `get_{access_level}v8{language}seasons{season_year}{season_type}standings.{format}`) and Rankings
   (`/{access_level}/v8/{language_code}/seasons/{season_year}/{season_type}/rankings.{format}`, operationId `get_{access_level}v8{language_code}seasons{season_year}{season_type}leaders.{format}-1`).
3. **Leaders** — call League Leaders
   (`/{access_level}/v8/{language_code}/seasons/{season_year}/{season_type}/leaders.{format}`, operationId
   `get_{access_level}v8{language_code}leaguehierarchy.{format}-1`) for category leaders with full player stats.
4. **Team detail** — for each team of interest call Seasonal Statistics
   (`/{access_level}/v8/{language}/seasons/{season_year}/{season_type}/teams/{team_id}/statistics.{format}`, operationId
   `get_{access_level}v8{language}seasons{season_year}{season_type}teams{team_id}statistics.{format}`).
5. **Refresh incrementally** — instead of re-pulling everything, call the Daily Change Log
   (`/{access_level}/v8/{language_code}/league/{year}/{month}/{day}/changes.{format}`, operationId
   `get_new-endpoint`) for the IDs updated on a date and re-fetch only those.

## Conventions (apply to every step)

- Base URL: `https://api.sportradar.com/unrivaled/` — every path starts with your key's
  `{access_level}` (`trial` or `production`), then `v8`, then the language code `en`.
- Auth: send your Sportradar API key in the `x-api-key` header on every request.
- Choose the response format with the path suffix `.json` or `.xml`; TLS 1.2+ is required.
- No pagination anywhere: each feed returns all available data in one response.
- Honor the `cache-control: max-age` response header before re-polling a feed; live game feeds
  shorten their TTL while a game is in progress.
- Timestamps are UTC ISO 8601; date-only fields follow local league convention.
- Unrivaled specifics: expect the `elam` period type with `clock_type`/`clock_directional`
  fields, weighted free throws (`free_throw_type="weighted"`, `weighted_value`), and
  `awardedpoint` events in 1v1 tournament games.
