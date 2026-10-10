---
name: prospect-audit
description: Audits a business the agency does not manage yet, from public data, through the Agency MCP connector — Google Business Profile completeness, reviews against nearby competitors, the website, tracking tags and map-pack position for its core searches — then publishes a branded page the agency can send. Use when asked to "audit [business] for a pitch", "run a prospect audit", "check how [business] shows up online before I reach out", or "make an audit I can send to [prospect]". Nothing needs to be connected.
---

# Prospect audit

The pitch tool. In a few minutes: a scorecard, the three fixes most worth making in everyday words, and (on a yes) a branded page to send. Nothing is sent to the prospect by any tool.

## Run
1. Get the business name and city from the person; ask for the trade in two or three words if it is not obvious ("plumber", "fence contractor"), and the website if they know it.
2. `audit_prospect` with `name`, `city`, `trade` and, when known, `website`. Add `terms` only if the person named specific searches; otherwise the tool builds "service + city" terms. The speed check adds up to 40 seconds; keep it on unless they are in a hurry.
3. Read the result: `overall`, five `sections` (each with `score`, `status`, `summary`, `facts`), `top_fixes`, `competitors`, `not_checked`.

## Tell the person
- Lead with what the picture means, not the numbers: what a customer searching in that town sees, and the one thing that would change it most. Then the overall score and each section in one line.
- The three fixes, as plain actions ("get ten more reviews", "add hours and photos to the profile", "put a title on every page").
- Competitors by name with their review counts; the gap is the pitch.
- A section marked `unverified` claims nothing. Say "I could not verify the Google profile", never "they have no profile". The free plan allows 5 audits a month; the result says how many are left.

## Hard rules (each came from a real mistake)
- Never invent a number. If `facts` has no PageSpeed score, review count or search volume, do not state one.
- Score against the business's actual trade and city; if the category on Google looks wrong, say so as a finding, do not re-score it.
- Phone-number differences between the site and Google are never a finding (tracking numbers are normal).
- Do not contact the prospect, draft outreach to them, or look up personal details. The agency sends the page.

## The page to send
On a clear yes: `create_prospect_report` with the prospect's name and sections built from the audit — one section per area that was checked (heading, two or three `stats` such as the score and the key count, and `commentary` in everyday words), plus a final "What we'd fix first" section with the three fixes. Preview first; publish with `confirm: true` only after the person agrees. Return the link. Branding must be set on the dashboard's White label tab first, or the page goes out unbranded; the preview says if it is missing.

## When they sign
`create_client` with the same name and `website`, then `link_account` as access is granted. The report link keeps working; the client audit (`audit_client`) takes over from there.
