---
name: autopilot
description: Runs Autopilot on a client through the Agency MCP connector — the hands-off weekly pass that works leads and forms, tracking, the website and SEO, Google Ads and Business Profile, within the limits the agency set, on the person's own Claude plan. Use when the person says "run Autopilot on [client]", "turn on Autopilot", "put [client] on autopilot", "set up the weekly Autopilot task", asks what Autopilot did, learned or needs from them, or says how Autopilot should or should not behave. Premium plan.
---

# Autopilot

Autopilot works a client end to end so the agency (or the business owner) does not have to: leads and forms first, then tracking, the website and SEO, Google Ads, Business Profile and the report. It runs inside the person's own Claude, on their Claude plan, so there is no separate AI bill.

## Turn it on
1. `autopilot_set` for the client with `mode: "shadow"` first. Shadow investigates fully and reports what it would do, changing nothing. Leave `allow` out for the defaults (Ads clean-up and growth within the budget, tracking, SEO titles, new posts and pages published live, page text edits, one Business Profile post a week, reports). Forms are opt-in.
2. Run the first pass now (below) so they see it work.
3. When they agree with what shadow would do, `autopilot_set` with `mode: "on"`.
If the plan does not include Autopilot, the tool says so with the upgrade link: give them the link and offer to switch it on once they have upgraded.
When `get_started`, `create_client` or `link_account` returns `autopilot_offer`, make that offer once: first say what Autopilot would do for this client (`how_it_helps`, tied to any problem just found: does Autopilot fix it, or does it need the person, e.g. a missing permission), then ask whether to start it in shadow.

## Run it now ("run Autopilot on [client]")
1. `autopilot_start` with the client (and `started_by`, e.g. "the agency, on request"). It returns a briefing and `instructions`, and locks this conversation to that client and its allowed changes. Agency MCP checks every call: other clients are refused, and changes outside the limits are recorded as proposals.
2. Follow `instructions` exactly. They begin with Autopilot's own past changes, scored on leads: undo or correct anything that was followed by fewer or worse leads, and keep what worked.
3. Finish with `autopilot_finish`: the run_id, a short report (status, headline, at most 3 done, at most one needs_you, details), up to 3 `lessons`, and `changes` — the change log: every change made, why, and the measurable result expected. If anything was changed through a connector other than Agency MCP (a site's own WordPress connector), list it in `changes_elsewhere` so the next run can judge it.
4. Tell the person the headline, what changed, and the one thing that needs them, if any. Nothing else.
One client per conversation: run the next client after `autopilot_finish`.

## Every week, by itself
`autopilot_routine` gives the prompt for a weekly scheduled task in Claude (all clients in shadow or on, or one client). Offer to set it up: in Claude, create a scheduled task that runs once a week with the Agency MCP connector on, and paste the prompt.

## What it did, learned, and what needs the person
- `autopilot_status` with no client: everything Autopilot needs from the agency across clients (`needs_you`, each with its fix). With one client: its runs, open lead alerts, proposals waiting for a yes, what it has `learned` about the client, and what has worked across all Autopilot clients (`across_autopilot`). Use it for "what does Autopilot need from me" and "what has Autopilot learned".
- When a result carries `autopilot_needs_you`, mention it once per conversation: the count and the most urgent item, and offer to handle it.
- On the person's yes or no to a proposal: `autopilot_approve` with its id and `approve` (runs exactly that change, nothing else) or `decline`.
- `autopilot_log`: every change Autopilot made, why, the result it expected, and the measured result (leads before against after). Use it for "what did it change", "why did it do that", "did it work".
- `whole_picture`: the ads, the website and Analytics joined per page and per search (paid clicks against leads from every source and organic rank). Look at it before adjusting anything, by hand or with Autopilot.
- `ads_diagnose`: the Google Ads checks Autopilot must clear (tracking, bidding, reach lost to rank or budget, keyword quality, ad strength, ad text relevance, wasted searches). Ad text relevance reads every live headline and description: leftover or off-topic text is a problem whatever Google's ad strength says.
- The dashboard's Autopilot tab shows the same: what needs the person (with one-click approve or a prompt to run), and each client's runs, changes and reasons.

## How the agency wants it to work
- When the person says how Autopilot should or should not behave ("never touch the brand campaign", "write posts in German", "don't do that again"), record it with `autopilot_preference` (action `add`, one client or every client) and confirm it in a sentence. `list` shows everything it follows, stated and learned; `remove` drops one by key.
- Preferences come first in every run, ahead of the agency's notes, the client's own results, the approved playbook and what worked elsewhere. Autopilot also learns them from what the agency approves, declines and undoes.
- When the person declines a proposal or turns Autopilot off and the result carries `ask_why`, ask in one short line why, and record it with `autopilot_feedback`.

## Rules
- Before any change, check the whole picture for what it touches; every change-log reason cites at least two data sources.
- One change at a time: after a significant change (bidding, budget, a new ad or keywords, a page edit) the next one on the same account or page waits until it has been judged.
- Autopilot never spends more money, takes a page down, messages anyone or touches another client on its own; those come back as proposals.
- If it is blocked (a page builder it cannot edit, a missing login, a failing connector), that is its one ask, with the exact fix.
- It never rebuilds a site. If a rebuild would pay off, it says why; the person decides.
