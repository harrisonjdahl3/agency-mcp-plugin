---
name: seo-audit
description: Audits a client's organic search health using Search Console, Google Analytics and the live WordPress site through the Agency MCP connector. Use when asked how a client's SEO is doing, why traffic dropped, what ranks, what to fix first, or for a monthly SEO review. Also use for "which pages are losing clicks", "what queries are we close to page one on", or "audit this site". Not for paid search (use google-ads-manager) or for writing content (use seo-content).
---

# SEO audit

## Start with the facts, not memory
1. `list_clients` → find the client and note its `gsc_site_url` and `ga4_property_id`. If either is missing, say so and point to the dashboard → Clients to link it. Do not guess IDs.
2. Choose the period: default the last 28 days vs the 28 before. Name both ranges in the answer.

## Pull, in this order
- **Rankings and clicks** — `gsc_query` with dimensions `["query"]` for both periods (row_limit 500). Then `["page"]` for both periods.
- **Position 4–15 opportunities** — from the query rows, list terms with impressions ≥ 50 and position between 4 and 15: these move with on-page work.
- **Losers** — pages or queries whose clicks fell ≥ 30% period over period with impressions still present (a ranking problem) versus impressions also falling (a demand problem).
- **Site behaviour** — `ga4_report` with metrics `["sessions","engagedSessions","keyEvents"]` and dimension `["sessionDefaultChannelGroup"]`; then `["landingPagePlusQueryString"]` with `["sessions","keyEvents"]` limit 50 for Organic Search context.
- **Technical spot-check (WordPress sites only)** — `wp_check_site` for reachability and plugin health, `wp_list_posts` for stale or thin pages, `wp_get_post` on the top 3 organic landing pages to check title, H1, and whether a meta description and schema exist.

## Judge, then write
Score each finding by expected lead impact, not by SEO purity. Lead-generating service pages come first.

Output, in this order:
1. **Headline** — three numbers: clicks, impressions, leads from organic, each with the change.
2. **What moved and why** — the two or three biggest changes, with the query or page behind each.
3. **Quick wins (this week)** — position 4–15 terms mapped to the page that should own them, missing meta descriptions, pages without a clear H1, internal links to add.
4. **Bigger fixes** — thin or missing service pages, cannibalised queries, slow or broken pages.
5. **Next actions** — ready to hand to `seo-content` (page drafts) or to the agency.

## Rules
- Never present a number without its date range.
- A drop in impressions with stable position is demand or seasonality, not a penalty. Say which.
- If Search Console has under 16 days of data (a new property), say the baseline is still forming and keep conclusions light.
- Do not change anything on the site from this skill. Hand edits to `seo-content`, which previews before it writes.
