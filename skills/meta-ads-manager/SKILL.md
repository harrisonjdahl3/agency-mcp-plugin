---
name: meta-ads-manager
description: Reports on and manages a client's Meta (Facebook and Instagram) ads through the Agency MCP connector — spend, reach, clicks and conversions by campaign, pausing or enabling campaigns, changing daily budgets, and creating paused campaigns. Use for "how are the Facebook ads doing", "pause the Meta campaign", "what's our Meta spend", "set up a Meta campaign". Every change is previewed before it applies.
---

# Meta ads management

## Always
- `meta_list_accounts` first; use the `act_…` id it returns. Note whether the account can currently spend.
- `meta_list_pages` when a campaign needs a Page.
- Name the date range in every number.

## Reporting
`meta_insights` with `level: "campaign"` and either `date_preset` (`last_7d`, `last_30d`, `this_month`) or `since`/`until`. Report spend, reach, impressions, clicks, CTR, CPC and conversions per campaign. For "which ad is winning", rerun at `level: "ad"`.

## Changes — preview, confirm, apply
- `meta_set_campaign_status` (PAUSED or ACTIVE) — the preview shows the campaign's current state; activating resumes spend, say so.
- `meta_set_campaign_budget` — the preview shows the current and proposed daily budget; large increases are refused until acknowledged.
- `meta_create_campaign` — created paused, with objective and daily budget; ad sets and creatives are finished in Ads Manager.
Show the preview, get an explicit yes, then call again with `confirm: true`.

## Rules
- Never activate a campaign or raise a budget without an explicit instruction naming it.
- Report currency as the account's currency.
- If the account isn't linked to a client on the dashboard, the connector refuses; say how to link it.
