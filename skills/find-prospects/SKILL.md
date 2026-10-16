---
name: find-prospects
description: Finds local businesses an agency could win as clients, checks each one's website, ranks them by how much the agency could fix, and drafts a first email, a follow-up and a phone opener for the ones the agency picks, through the Agency Autopilot connector. Use when asked to "find prospects", "find 10 roofers in Denver", "who could I pitch in [area]", "get me leads for my agency" or "find businesses that need a website fix". Works in any country. Uses one prospect credit per business found and checked. Nothing is ever sent. The dashboard's Prospects tab runs the same search.
---

# Find prospects

From a trade and an area to a short list of businesses worth calling, each with the findings to open with and drafts ready to send from the agency's own email, phone or CRM. Nothing is sent by any tool.

## The flow: find → review → drafts → the agency sends
1. **Find.** `find_prospects` with `keyword` (the trade, e.g. "roofer") and `location` (city, region or postcode, any country). `count` defaults to 10 (max 20). Add `min_reviews` or `max_rating` only if the person asks. One credit per business found and checked. Skipped businesses, and sites we couldn't check, cost nothing.
   - If the result has `needs_confirm`, the run would use credits they bought. Tell them what it says ("uses up to N credits…") and call again with `confirm: true` only after they agree.
   - If `ran: false` with a reason (out of credits, or the daily limit), say it and give the Billing link it includes. Do not retry.
2. **Review with the person.** Show the ranked list, best first: name, area, rating and reviews, website, score, and the 1-3 findings. Keep each finding's wording. They are observations from the pages listed in `checked` ("We couldn't find an enquiry form on the pages we checked…"), so never turn them into absolutes. A prospect with `site_note` couldn't be checked (often bot protection): say so and claim nothing about its website. Give the published `emails`, each with the page it came from, and the phone. Ask which ones to prepare.
3. **Drafts.** `prospect_drafts` with the `find_id` and the chosen `ids`. For each one you get the shareable audit page (`report_url`), the first email (a short question with no link), the follow-up for when they reply yes (with the link), and a phone opener. Hand-offs:
   - `text` (default): show the drafts to copy.
   - `csv`: a download link on the dashboard.
   - `highlevel`: adds each prospect to the agency's own HighLevel sub-account with the drafts as a note. Preview first; `confirm: true` only after the person agrees. Needs HighLevel writes switched on.
4. **The agency sends.** From their own email, phone or CRM. Suggest calling first where the result's `compliance` note says calling is safest.

## Hard rules
- Never send, schedule or sequence anything to a prospect. There is no tool for it, and do not look for one.
- Use only contact details the business publishes on its own site or Google listing (the result's `emails`, `phone`). Never guess, construct or look up a person's address.
- Never state a finding the result doesn't contain, and never make one stronger ("their site is down" when it says "didn't load when we checked twice").
- Always pass on the `compliance` note for the country.
- Credits: monthly ones are used first, then the 100 welcome credits, then bought packs. The result's last line (`credits`) is the balance; quote it as it is.

## After
For a deeper look at one business before the pitch, `audit_prospect` with its website (see the prospect-audit skill). When they sign: `create_client` with the same name and website.
