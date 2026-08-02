# Unrivaled Basketball

Unrivaled is a U.S. professional women's 3x3 basketball league co-founded by Napheesa Collier and Breanna Stewart, playing a condensed winter season with six clubs and a midseason 1-on-1 tournament. Backed by bessemer-venture-partners — https://www.unrivaled.basketball/

The league's official real-time data API — the **Unrivaled API v8** — is operated by Sportradar as the league's Official Data Provider: 23 GET feeds covering the league hierarchy, teams, players, games (boxscore, play-by-play, summary), seasons, series, tournaments, injuries, transfers, standings, rankings, and leaders, with x-api-key authentication and JSON/XML responses. Docs: https://developer.sportradar.com/basketball/reference/unrivaled-overview

## Artifacts

- `apis.yml` — APIs.json profile for the company and the Unrivaled API
- `openapi/` — Unrivaled API v8 OpenAPI, assembled from the per-endpoint definitions on Sportradar's reference pages
- `overlays/` — API Evangelist overlay repairing auto-generated summaries and adding tags
- `authentication/`, `conventions/`, `conformance/`, `lifecycle/`, `changelog/`, `sandbox/`, `data-model/` — captured API semantics
- `mcp/` — candidate MCP tool surface derived from the OpenAPI (no official server exists)
- `skills/` — generated Agent Skills for live-game tracking, roster lookup, and season statistics
- `llms/`, `well-known/`, `security/` — discovery and security surface
