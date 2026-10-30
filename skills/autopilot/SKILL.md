---
name: autopilot
description: Runs Autopilot on a client through the Agency Autopilot connector — the hands-off weekly pass that works leads and forms, tracking, the website and SEO, Google Ads and Business Profile, within the limits the agency set, on the person's own AI assistant plan. Use when the person says "run Autopilot on [client]", "turn on Autopilot", "put [client] on autopilot", "set up the weekly Autopilot task", asks what Autopilot did, learned or needs from them, or says how Autopilot should or should not behave. Premium plan.
---

# Autopilot

Autopilot works a client end to end so the agency (or the business owner) does not have to: leads and forms first, then tracking, the website and SEO, Google Ads, Business Profile and the report. It runs inside the person's own AI assistant, on their own plan, so there is no separate AI bill.

## Set it up first (once per client, and once per agency)
`autopilot_setup_questions` returns what's still unanswered: the agency's questions only the first time, then just the client's (what it sells, as ticked services taken from its Business Profile and service pages, including Meta ads; where its customers come from; what counts as a win; competitors it knows). Every question has a "Something else" box for the person's own words. Ask the unanswered ones in one message with the options and the default, then `autopilot_setup_save`. Never re-ask what's saved.

## Turn it on
1. `autopilot_set` for the client with `mode: "shadow"` first. Shadow investigates fully and reports what it would do, changing nothing. Leave `allow` out for the defaults (Ads clean-up and growth within the budget, tracking, SEO titles, new posts and pages published live, page text edits, one Business Profile post a week, reports). Forms are opt-in.
2. Run the first pass now (below) so they see it work.
3. When they agree with what shadow would do, `autopilot_set` with `mode: "on"`.
If the plan does not include Autopilot, the tool says so with the upgrade link: give them the link and offer to switch it on once they have upgraded.
When `get_started`, `create_client` or `link_account` returns `autopilot_offer`, make that offer once: first say what Autopilot would do for this client (`how_it_helps`, tied to any problem just found: does Autopilot fix it, or does it need the person, e.g. a missing permission), then ask whether to start it in shadow.

## Run it now ("run Autopilot on [client]")
1. `autopilot_start` with the client (and `started_by`, e.g. "the agency, on request"). It returns a briefing and `instructions`, and locks this conversation to that client and its allowed changes. Agency Autopilot checks every call: other clients are refused, and changes outside the limits are recorded as proposals.
2. Follow `instructions` exactly. They begin with Autopilot's own past changes, scored on leads: undo or correct anything that was followed by fewer or worse leads, and keep what worked.
3. Finish with `autopilot_finish`: the run_id, a short report (status, headline, at most 3 done, at most one needs_you, details), up to 3 `lessons`, and `changes` — the change log: every change made, why, and the measurable result expected. If anything was changed through a connector other than Agency Autopilot (a site's own WordPress connector), list it in `changes_elsewhere` so the next run can judge it.
4. Tell the person in this shape: "N real problems." Each one in a plain sentence (what's wrong and why it matters), how to fix it in one line, and "Want Autopilot to do it?" (or "This one needs you: …"). Work already done is one short line each with a link. If nothing is wrong, one line saying so. No metrics, no internal reasoning, and never a run number or other internal id.
One client per conversation: run the next client after `autopilot_finish`.

## Every week, by itself
`autopilot_routine` gives the weekly prompt and how to run it in this assistant (all clients in shadow or on, or one client). Offer to set it up: where this assistant supports scheduled tasks with connectors, create one that runs once a week with the Agency Autopilot connector on, and paste the prompt; otherwise, run it with a weekly ask.

## What it did, learned, and what needs the person
- `autopilot_status` with no client: everything Autopilot needs from the agency across clients (`needs_you`, each with its fix). With one client: its runs, open lead alerts, proposals waiting for a yes, what it has `learned` about the client, and what has worked across all Autopilot clients (`across_autopilot`). Use it for "what does Autopilot need from me" and "what has Autopilot learned".
- When a result carries `autopilot_needs_you`, mention it once per conversation: the count and the most urgent item, and offer to handle it.
- On the person's yes to a proposal: `autopilot_proposal_get` with its id gives the exact tool call; make that call yourself (its own preview and confirm), then `autopilot_proposal_done`. On a no: `autopilot_decline`.
- `autopilot_log`: every change Autopilot made, why, the result it expected, and the measured result (leads before against after). Use it for "what did it change", "why did it do that", "did it work".
- `whole_picture`: the ads, the website and Analytics joined per page and per search (paid clicks against leads from every source and organic rank). Look at it before adjusting anything, by hand or with Autopilot.
- `keyword_research`, `keyword_map_get`, `keyword_map_save`, `keyword_map_approve`: the client's keyword map, the SEO strategy Autopilot works from: groups of the searches that bring the business its customers, each with the page that should rank, measured weekly (top 3, page 1, page 2, not ranking, split between two of its pages, no page yet) with the best opportunities first. Every save is a draft: show the person the plan as a short table (page, its main search, searches a month) and ask for approval with `keyword_map_approve`; the approved plan stays in force until the new one is approved. In a run with nobody watching, the draft goes to needs-you instead.
- `site_health`: speed on the money pages, page faults (cut-off sentences, broken links, missing H1, long titles), indexing (pages Google hasn't indexed, with its reason) and Business Profile health, each week against the last. Autopilot reads it every run and fixes or proposes; `refresh: true` with a `url` re-checks one page after a change.
- `competitor_pages`: what the pages Google Maps ranks for a service have in common, against the client's page, with an outline. Autopilot uses it before drafting or reworking a page.
- `landing_evidence`: 90 days of Ads, Analytics, Search Console and form leads per page, and the page that converts paid visitors. Every ad, sitelink or landing-page change starts here.
- `ads_diagnose`: the Google Ads checks Autopilot must clear (tracking, bidding, reach lost to rank or budget, keyword quality, ad strength, ad text relevance, wasted searches). Ad text relevance reads every live headline and description: leftover or off-topic text is a problem whatever Google's ad strength says.
- The dashboard's Autopilot tab shows the same: what needs the person (with one-click approve or a prompt to run), and each client's runs, changes and reasons.

## How the agency wants it to work
- When the person says how Autopilot should or should not behave ("never touch the brand campaign", "write posts in German", "don't do that again"), record it with `autopilot_preference` (one client or every client) and confirm it in a sentence. `autopilot_preferences` shows everything it follows, stated and learned; `autopilot_preference_remove` drops one by key.
- Preferences come first in every run, ahead of the agency's notes, the client's own results, the approved playbook and what worked elsewhere. Autopilot also learns them from what the agency approves, declines and undoes.
- When the person declines a proposal or turns Autopilot off and the result carries `ask_why`, ask in one short line why, and record it with `autopilot_feedback`.

## Rules
- Autopilot is a full-stack SEO and ads agency for the client: the goal is ranking for the searches that bring its customers, and the leads, jobs and sales that follow, not activity.
- Before any change, check the whole picture for what it touches; every change-log reason cites at least two data sources.
- One change at a time: after a significant change (bidding, budget, a new ad or keywords, a page edit) the next one on the same account or page waits until it has been judged.
- Autopilot never spends more money, takes a page down, messages anyone or touches another client on its own; those come back as proposals.
- If it is blocked (a page builder it cannot edit, a missing login, a failing connector), that is its one ask, with the exact fix.
- It never rebuilds a site. If a rebuild would pay off, it says why; the person decides.
