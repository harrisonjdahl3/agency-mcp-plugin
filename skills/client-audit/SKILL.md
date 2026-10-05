---
name: client-audit
description: Audits one client through the Agency MCP connector and returns a scorecard of what to fix first — Google Ads (conversion tracking, keywords and search terms spending with no conversions, impression share, extensions), Meta ads (leads recorded, campaigns spending without leads, ad fatigue, click-through), Search Console (rankings within reach, titles nobody clicks) and Analytics (whether leads are measured). Then applies the safe Google Ads fixes in one confirmed step. Use for "audit Acme Fence", "what should we fix first", "where is the money going", "do it all", and as the first thing to run on a new client.
---

# Client audit

## Run it
1. `list_clients` for the exact client name. No client yet: `suggest_clients`, then `create_client` (preview, then confirm).
2. `audit_client` with that name. One call reads every linked platform; nothing changes.

## Read the result
- `scorecard`: twelve areas, each `ok`, `warning` or `critical`. Three other statuses are not failures: `not_linked` (that platform is not linked to the client), `unavailable` (it could not be read; the summary says why) and `blocked` (see below).
- `top_actions`: up to three fixes, most valuable first. Each carries `why`, a `monthly_amount` where money is involved, the `tool` that makes the fix, and `items` with the ids that tool needs. `more_actions` holds the rest.
- Google Ads and Meta figures cover the last 30 days; Search Console and Analytics the last 28. Say the period with every number.

## Report it
Lead with how many areas need attention, then the critical ones, then warnings, one line each with its number. Then the top actions as a short numbered list with amounts in the account's currency. Do not list the areas that are fine one by one; say they are fine in one sentence.

## Tracking comes first
If conversion tracking is `critical` or `warning`, keyword and search-term spend come back `blocked`. That is deliberate: with no working tracking, zero conversions means unmeasured, not wasted. Do not call that spend waste and do not pause anything on it. The first action is to fix tracking (`ads_create_conversion_action`, `wp_tracking_status`, `wp_install_tracking`). The same rule applies to Meta when no leads are recorded.

## "Do it all"
`apply_audit_fixes` applies the two safe, reversible Google Ads fixes together:
- pauses every keyword the audit flags as spending with no conversions (`pause_keywords`, on by default);
- adds the search terms the person chose as phrase-match negatives (`negative_terms`).

How to use it:
1. Before calling it, show the flagged search terms and ask which are irrelevant. A term with no conversions may still be a real customer; only the ones the person agrees are irrelevant go in `negative_terms`. Never send them all by default.
2. Call it without `confirm` to get the preview: the keywords, the terms and what each cost. Show that list.
3. On an explicit yes, call again with `confirm: true`.
4. Report what was paused and added, anything listed under `failed` or `refused`, and that each keyword can be turned back on with `ads_set_keyword_status`.

It accepts only items the audit flags at that moment, so a term it lists under `refused` was not flagged; leave it. It does not change budgets, extensions or anything on Meta.

## The other fixes are separate steps
- More budget for a campaign that converts and runs out: `ads_campaign_state`, then `ads_set_campaign_budget`. This spends more money; ask first.
- Missing extensions: `ads_add_extensions`, with wording the person approves.
- A Meta campaign spending without leads: `meta_set_campaign_status` to pause it. A traffic or awareness campaign is not expected to produce leads; check what it is for before proposing a pause.
- Ad fatigue on Meta: new creative is uploaded in Meta Ads Manager; say so.
- Rankings between 5 and 20, and low click-through titles: `wp_get_post` then `wp_set_seo` on the page behind the query.
- Leads not measured in Analytics: `ga4_create_key_event`, then `wp_tracking_status`.

Each of these previews first and applies only on confirmation.

## Afterwards
Tell the person that `review_changes` will score these changes once seven full days have passed, and offer to check then.
