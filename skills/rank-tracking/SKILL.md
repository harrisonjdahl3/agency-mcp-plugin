---
name: rank-tracking
description: Tracks where a client ranks on Google through the Agency MCP connector — the Search Console position for up to ten search terms every day, and the map-pack (Google Maps) position every Monday — and reads the movers back. Use when asked to "track rankings for [client]", "where does [client] rank for [term]", "are we moving up for [search]", "set up rank tracking", or when a client report needs a rankings section. Needs the client's Search Console site linked for positions; a city for the map pack.
---

# Rank tracking

Two tools, one number the client asks for every week: where they rank.

## Set it up
1. `list_clients` for the exact client name. Positions need the client's Search Console site linked (`link_account` if not); the map pack needs nothing linked, only a city.
2. `rank_track` with `client`, and:
   - `terms`: up to ten searches as a customer types them ("fence installation boise", "deck builder near me"). Leave it out on the first call and the tool picks the client's best Search Console queries itself.
   - `city`: "Boise, ID" style, once. The tool finds the client's Business Profile on the map and checks it every Monday. If it cannot match the business by name it says so; do not guess a different name.
   - `remove`: terms to stop tracking.
3. Read back `first_reading` and say it in words. The first reading has no "week ago"; movement starts next week.

## Read it
`rank_report` with `client` → `terms[]` with `position` (Search Console average over the latest 7-day window), `week_ago`, `month_ago`, `change_week`, `clicks_7d`, `impressions_7d`, `map_rank` (Monday's check; null = not in the top 20; absent = never checked) and a `verdict` (up / down / flat / new / not_showing / no_data). `summary[]` is the one-line version per term.

Tell the person: movers first (up and down by a full place or more), then the terms not showing, then the map pack. A smaller position number is higher on the page. Search Console data lags two days; say "as of" the `as_of` date.

## Rules
- Never state a position the tool did not return. `no_data` means no reading yet, not a bad rank.
- `not_showing` means the site had no impressions for that term in the window — not that it is "banned" or "deindexed".
- Ten terms per client; to add an eleventh, remove one. Pick terms with buyer intent and the city, not single words.
- The Monday email every agency gets carries the same movers; the dashboard does not have a rankings tab yet.

## In a report
For `create_client_report`, a rankings section is a `table` with columns Search · Position · Last week · Map pack and one row per term from `rank_report`, plus one sentence of commentary on the movers.
