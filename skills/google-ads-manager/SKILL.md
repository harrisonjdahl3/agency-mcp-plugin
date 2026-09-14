---
name: google-ads-manager
description: Manages a client's Google Ads through the Agency MCP connector — performance reports, search-term hygiene, negative keywords, budgets, pausing or enabling, and building new search campaigns with ad groups, keywords and responsive search ads. Use for "how are the ads doing", "what did we spend", "add negatives", "raise the budget", "pause that campaign", "build a campaign for X". Every write is validated by Google and previewed before it applies. Requires the Google Ads tools on the connector.
---

# Google Ads management

## Always
- `ads_list_accounts` first; take `customer_id` from it, never from memory.
- Name the date range in every number.
- Costs come back in micros: divide by 1,000,000. Say "$" figures only.

## Reporting (`ads_report`, GAQL)
- Campaigns: `SELECT campaign.name, campaign.status, metrics.cost_micros, metrics.impressions, metrics.clicks, metrics.conversions, metrics.cost_per_conversion FROM campaign WHERE segments.date BETWEEN '<start>' AND '<end>' ORDER BY metrics.cost_micros DESC`
- Search terms: `FROM search_term_view` with `search_term_view.search_term, metrics.cost_micros, metrics.clicks, metrics.conversions`.
- Keywords: `FROM keyword_view` with `ad_group_criterion.keyword.text, ad_group_criterion.keyword.match_type, metrics.*`.
- State of one campaign, including whether its budget is shared: `ads_campaign_state`.
Report spend, clicks, conversions and cost per conversion; then the three search terms wasting the most money and the three earning the most.

## Changes — preview, confirm, apply
Every write tool runs as a validated preview unless `confirm: true`. Show the preview, get an explicit yes, then call again with `confirm: true`.
- Negatives: `ads_add_negative_keywords` (campaign level). Build the list from search terms with spend and no conversions, and from informational or DIY terms.
- Budgets: `ads_set_campaign_budget`. Increases over 50% in one step are refused by the connector until the person acknowledges the size; say so and ask before retrying.
- Status: `ads_set_campaign_status`, `ads_set_ad_status`, `ads_set_keyword_status`. Enabling resumes spend; say that in the preview.

## Building a search campaign
1. Research: `ads_find_locations` for the service area; `ads_keyword_ideas` for volumes.
2. `ads_create_campaign` — daily budget, geo targets, bidding. It previews with the monthly equivalent; confirm to create. Campaigns are created **paused**.
3. `ads_create_ad_group` → `ads_add_keywords` (phrase match by default, exact for the strongest terms) → `ads_create_ad` (responsive search ad: 8–15 headlines, 4 descriptions, final URL on the client's site).
4. `ads_add_negative_keywords` with the informational list.
5. Tell the person the campaign is paused and what must be checked before enabling: conversion tracking, landing page, phone number.

## Rules
- Never enable spend without an explicit instruction that names the campaign.
- If a preview comes back with a Google validation error, report the field it names and fix the input; do not retry blindly.
- If the account is not linked to a client on the dashboard, the connector refuses; say how to link it.
