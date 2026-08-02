---
name: Track a live Unrivaled game
description: Find today's games and poll real-time boxscore and play-by-play for one game.
api: openapi/unrivaled-basketball-openapi.yml
operations:
- get_access-level-v8-language-code-games-year-month-day-schedule-format
- get_{access_level}v8{language_code}league{year}{month}{day}injuries.{format}-1-1
- get_{access_level}v8{language_code}games{game_id}boxscore.{format}-1
- get_{access_level}v8{language_code}games{game_id}pbp.{format}-1
generated: '2026-07-21'
method: generated
---

## Steps

1. **Find today's games** — call the Daily Schedule feed
   (`/{access_level}/v8/{language_code}/games/{year}/{month}/{day}/schedule.{format}`, operationId
   `get_access-level-v8-language-code-games-year-month-day-schedule-format`) with year/month/day. Collect each game's
   GUID `id`, `home_team`, `away_team`, and `status`.
2. **Poll the boxscore** — call Game Boxscore
   (`/{access_level}/v8/{language_code}/games/{game_id}/boxscore.{format}`, operationId
   `get_{access_level}v8{language_code}league{year}{month}{day}injuries.{format}-1-1`) with the `game_id` for scores by quarter and team leaders.
3. **Stream the narrative** — call Game Play-by-Play
   (`/{access_level}/v8/{language_code}/games/{game_id}/pbp.{format}`, operationId
   `get_{access_level}v8{language_code}games{game_id}boxscore.{format}-1`) for every possession and event. Watch for
   Unrivaled-specific events: `elam` period, weighted free throws, `awardedpoint` (1v1 only).
4. **Close out** — when `status` reaches `closed`, fetch Game Summary
   (`/{access_level}/v8/{language_code}/games/{game_id}/summary.{format}`, operationId
   `get_{access_level}v8{language_code}games{game_id}pbp.{format}-1`) for final team and player statistics, including
   `free_throw_points` totals.

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
