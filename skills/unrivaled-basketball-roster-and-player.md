---
name: Look up an Unrivaled roster and player profile
description: Walk the documented League Hierarchy -> Team Profile -> Player Profile retrieval path.
api: openapi/unrivaled-basketball-openapi.yml
operations:
- get_{access_level}v8{language_code}leagueinjuries.{format}-1
- get_{access_level}v8{language_code}seasons{season_year}{season_type}teams{team_id}statistics.{format}-1-1-1
- get_{access_level}v8{language}teams{team_id}profile.{format}
- get_{access_level}v8{language}players{player_id}profile.{format}
generated: '2026-07-21'
method: generated
---

## Steps

1. **List the clubs** — call League Hierarchy
   (`/{access_level}/v8/{language_code}/league/hierarchy.{format}`, operationId
   `get_{access_level}v8{language_code}leagueinjuries.{format}-1`) or Teams
   (`/{access_level}/v8/{language_code}/league/teams.{format}`, operationId `get_{access_level}v8{language_code}seasons{season_year}{season_type}teams{team_id}statistics.{format}-1-1-1`)
   and locate the team GUID.
2. **Open the roster** — call Team Profile
   (`/{access_level}/v8/{language}/teams/{team_id}/profile.{format}`, operationId
   `get_{access_level}v8{language}teams{team_id}profile.{format}`) with `team_id`; the full active roster with
   player GUIDs is returned.
3. **Profile the player** — call Player Profile
   (`/{access_level}/v8/{language}/players/{player_id}/profile.{format}`, operationId
   `get_{access_level}v8{language}players{player_id}profile.{format}`) with `player_id` for biographical info and
   seasonal statistics. Player GUIDs are persistent across Sportradar leagues
   (WNBA, NCAAWB), so they can join data across competitions.

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
