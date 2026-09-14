---
name: client-report
description: Produces a plain-English weekly or monthly client report across every channel the client has linked — organic search, website, Google Ads, Meta ads, CRM leads and won work — and publishes it as a branded shareable page through the Agency MCP connector. Use for "send Acme their monthly report", "how did the client do last month", "build the weekly report". Starts from client_overview and writes commentary a business owner understands.
---

# Client report

## Pull
1. `list_clients` → the client name exactly as stored.
2. `client_overview` with the period (`days`: 7 for weekly, 28 or 30 for monthly) and `compare: true`. It returns every linked source for this period and the previous one of the same length under `previous`, and names the sources that are not linked; do not pull those separately. Every "up from" / "down from" comes from `previous`.
3. For detail the overview lacks, add targeted calls: `gsc_query` (top queries and pages), `ads_report` (search terms), `meta_insights` (campaigns), `highlevel_opportunities` (won work).

## Write
Audience: the business owner. Each section is a heading, two to four headline numbers with the change versus the previous period, one short table if it helps, and commentary in plain English: what happened, why, and what the agency will do next. No jargon, no hedging.

Suggested sections, only those with data:
- **Leads and jobs won** (from the CRM when linked; this is the number the client cares about)
- **Google Ads**: spend, clicks, leads, cost per lead
- **Meta ads**: spend, reach, leads
- **Getting found on Google**: clicks, impressions, top queries, position changes
- **Your website**: visits, leads, top landing pages
- **Next steps**: three concrete actions

## Publish
`create_client_report` with `client`, `title`, `period_label` and the `sections` (heading, stats with `sub` for the change, optional table, commentary). Return the link. Branding must be set on the dashboard first, or the report goes out unbranded; say so if the result shows no branding.

## Rules
- Every number carries its period.
- Never hide a bad month. State it, explain it, say the fix.
- If a source failed to read, tell the agency privately and leave the section out of the client-facing report.
