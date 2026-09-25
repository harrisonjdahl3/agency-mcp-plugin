---
name: local-seo-gbp
description: Manages a client's Google Business Profile through the Agency MCP connector — locations, reviews and owner replies, posts, and performance (calls, direction requests, website clicks, impressions). Use for "reply to that review", "any new reviews", "how many people found us on Maps", "post an update to our profile". Business Profile is a separate, optional sign-in on the Agency MCP dashboard ("Connect Business Profile"); if the tools report it is not connected, send the person there.
---

# Local SEO — Google Business Profile

## Availability
Business Profile uses its own sign-in, separate from the main Google connection. On the Agency MCP dashboard the person opens the **Business Profile** tab and clicks **Connect Business Profile**, signing in with the Google account that manages their clients' profiles (the one they use at business.google.com), keeping the Business Profile box ticked. If a `gbp_*` tool answers that Business Profile is not connected, say exactly that and point them to the tab; if the tools are absent from the list altogether, offer the Search Console and Analytics parts of local SEO instead (`seo-audit`).

Then link each location to its client: `gbp_list_locations` gives the `gbp_location_id`, which goes in the client's **Business Profile location** field under **Clients** so reviews and posts resolve by client name.

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
