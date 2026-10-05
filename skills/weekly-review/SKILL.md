---
name: weekly-review
description: Reviews the changes an agency confirmed through the Agency MCP connector and what happened afterwards — each Google Ads or Meta ads change with the seven days before set against up to seven days after, marked win, loss, flat or too new to judge. Use for "review last week's changes", "did that work", "what worked", "what should we undo", a Monday review, or before reporting to a client on what was changed.
---

# Weekly change review

## Run it
`review_changes` with `days` (14 by default, up to 60) and, for one client, `client` with the exact name from `list_clients`. It reads only.

## What comes back
- `totals`: how many changes, and how many were wins, losses, flat, too new, without data, or not measured.
- `changes`, newest first. Each has the date, the platform, what was changed, the client, a `verdict` and a `why` that quotes both weeks. Measured changes also carry `before` and `after` (cost, clicks, impressions, conversions) and `measured_on` (campaign, ad group or whole account).

## Verdicts
- `win` / `loss` / `flat`: judged on conversions when there are any, otherwise on click-through rate and cost.
- `too_new`: fewer than seven full days since the change. Say when it can be judged; do not guess early.
- `no_data`: nothing served in either week.
- `not_measured`: listed for the record. Only Google Ads and Meta ads changes are measured; WordPress, Analytics, Search Console and CRM changes are not.
- `low_confidence`: another change hit the same campaign within seven days, so the two cannot be told apart. Say so rather than crediting either.
- "Small numbers" in `why` means treat it as a hint, not a result.

## Report it
One line of totals, then wins, then losses, then what is too new, each as: what was changed, cost before and after, conversions before and after. Name the period. Costs are in each account's own currency.

A verdict says what moved after a change, not that the change caused it. Keep that distinction when summarising for a client.

## What to do with it
- A loss on a budget or bid change: propose reversing it with the same tool that made it (`ads_set_campaign_budget`, `ads_set_bidding_strategy`, `meta_set_campaign_budget`). Preview, then confirm.
- A loss on a paused keyword: `ads_set_keyword_status` with `ENABLED`.
- A win: leave it, and say what it suggests trying next.
- Nothing listed: only changes confirmed through Agency MCP appear. Changes made directly in Google Ads or Meta Ads Manager are not in this review.

## Rules
- Do not reverse anything without an explicit yes.
- Do not judge a change marked `too_new`.
