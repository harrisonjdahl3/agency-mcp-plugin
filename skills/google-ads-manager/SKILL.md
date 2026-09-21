---
name: google-ads-manager
description: Manages a client's Google Ads through the Agency MCP connector — performance reports, search-term hygiene, negative keywords, budgets, bidding strategies, conversion tracking set-up, extensions (sitelinks, callouts, snippets, call assets), pausing or enabling, and building new search campaigns with ad groups, keywords and responsive search ads. Use for "how are the ads doing", "what did we spend", "add negatives", "raise the budget", "pause that campaign", "build a campaign for X". Every write is validated by Google and previewed before it applies. Requires the Google Ads tools on the connector.
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

## Conversion tracking — check before anything else
`ads_list_conversion_actions`. If there is no ENABLED primary action, the account is blind: every campaign reports 0 conversions no matter how many leads arrive, and Smart Bidding has nothing to learn from. Say that first. Then offer `ads_create_conversion_action` (WEBPAGE for a form / thank-you page — it returns the tag to install; AD_CALL for calls from the ad; WEBSITE_CALL for calls from the site) and tell the person exactly where the tag goes. On a WordPress site connected here you can install it yourself with `wp_install_tracking`: `ads_id` is the `AW-…` from the tag, `conversion_label` is the part after the slash in `send_to`, and `conversion_path` is the thank-you page. It installs by ID only and previews the exact tags first. `ads_update_conversion_action` fixes status, primary flag, name or value.

## Bidding
`ads_campaign_bidding` shows the strategy and the last 30 days of conversions. Rules of thumb for a local-service account: under ~30 conversions a month, Maximize conversions with no target, or Maximize clicks with a CPC ceiling; a target CPA only once volume exists, set near the recent actual cost per lead. `ads_set_bidding_strategy` previews the before/after and warns when a target is premature.

## Extensions
`ads_list_extensions` lists sitelinks, callouts, structured snippets and call assets with a `gaps` list. A finished search campaign has 4+ sitelinks (service pages), 4+ callouts (licensed, free estimates, financing, warranty), one structured snippet (Services: …) and a call asset with the client's lead line. `ads_add_extensions` adds them per campaign or account-wide; `ads_remove_extension` retires an old one. A phone number change is add the new, then remove the old.

## Building a search campaign
1. Research: `ads_find_locations` for the service area; `ads_keyword_ideas` for volumes.
2. `ads_create_campaign` — daily budget, geo targets, bidding. It previews with the monthly equivalent; confirm to create. Campaigns are created **paused**.
3. `ads_create_ad_group` → `ads_add_keywords` (phrase match by default, exact for the strongest terms) → `ads_create_ad` (responsive search ad: 8–15 headlines, 4 descriptions, final URL on the client's site).
4. `ads_add_negative_keywords` with the informational list.
5. `ads_add_extensions`: sitelinks, callouts, a structured snippet and the call asset.
6. Tell the person the campaign is paused and what must be checked before enabling: conversion tracking (`ads_list_conversion_actions`), landing page, phone number.

## Rules
- Never enable spend without an explicit instruction that names the campaign.
- If a preview comes back with a Google validation error, report the field it names and fix the input; do not retry blindly.
- If the account is not linked to a client on the dashboard, the connector refuses; say how to link it.
