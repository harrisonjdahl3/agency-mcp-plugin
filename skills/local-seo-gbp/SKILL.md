---
name: local-seo-gbp
description: Manages a client's Google Business Profile through the Agency MCP connector — locations, reviews and owner replies, posts, and performance (calls, direction requests, website clicks, impressions). Use for "reply to that review", "any new reviews", "how many people found us on Maps", "post an update to our profile". Requires the Business Profile tools, which appear on the connector once Google has granted the agency's project access; until then tell the person the tools are not enabled yet.
---

# Local SEO — Google Business Profile

## Availability
The `gbp_*` tools register on the connector only after Business Profile API access and the `business.manage` scope are live for the deployment. If they are absent from the tool list, say so plainly and offer the Search Console and Analytics parts of local SEO instead (`seo-audit`).

## Reads
- `gbp_list_locations` first; use its `gbp_location_id`.
- Reviews: `gbp_reviews` — rating, text, whether replied. Lead with unanswered reviews.
- Performance: `gbp_performance` for a date range — impressions on Maps and Search, calls, website clicks, direction requests, with daily values.
- Posts: `gbp_posts`.

## Writes — preview, confirm, apply
- `gbp_reply_review`: draft the reply, show it, apply only on `confirm: true`. Thank by name, address the specific point, never argue, invite the conversation offline for complaints.
- `gbp_create_post`: 80–150 words, one clear call to action, optional photo URL; preview, then confirm.

## Rules
- Replies are public under the business name; never post one the person has not read.
- Report performance with the period and compare to the previous period.
