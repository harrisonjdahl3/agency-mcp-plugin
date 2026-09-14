---
name: keyword-research
description: Researches keywords for a client using Google Ads planning data (volumes, competition, bid ranges) joined with what the site already ranks for in Search Console. Use when onboarding a client, planning new service pages, building a campaign, or asked "what should we target", "keyword ideas for X", "what are people searching for". Produces a prioritised keyword map by page. Requires the Google Ads tools on the Agency MCP connector.
---

# Keyword research

## Inputs to establish first
- The client (`list_clients`), its Google Ads account (`ads_list_accounts` → `customer_id`), and its Search Console site if linked.
- The service area: `ads_find_locations` for the city or county the client serves; keep the geo target ID.
- Seeds: the client's services in the words a customer would use ("cedar fence installation", "deck builder"), and the client's site URL.

## Pull
1. `ads_keyword_ideas` with `seeds`, `url` and `geo_target_ids`. Keep: keyword, monthly volume, competition, top-of-page bid range.
2. If Search Console is linked: `gsc_query` (last 90 days, dimension `query`, row_limit 1000) to see what the site already earns impressions for.
3. Join the two lists on the keyword text. Mark each keyword: **already ranking (position and clicks)**, **ranking but weak (position > 10)**, or **new**.

## Prioritise
Score = intent × volume × winnability.
- Intent: "near me", "cost", "installers", "company", city names → high. "how to", "DIY", "ideas" → informational (blog only, or negatives for ads).
- Winnability: existing impressions and a page that already matches beat brand-new terms.
- Local: prefer the geo-scoped volume over national numbers.

## Output
A keyword map, one row per target: keyword · monthly volume · competition · bid range · current position (or "new") · **page that should own it** (existing URL or "new page: <title>") · priority (1–3).
Then:
- Three to five **page briefs** for the top new pages: working title, primary keyword, 3–5 secondaries, questions to answer.
- A **negative list** for paid search: informational and DIY terms found along the way.

## Rules
- Volumes are estimates; say so once, then stop hedging.
- Never invent a keyword's volume. If `ads_keyword_ideas` returns nothing for a seed, report that and try broader seeds.
- Hand page briefs to `seo-content`; hand the negatives to `google-ads-manager`.
