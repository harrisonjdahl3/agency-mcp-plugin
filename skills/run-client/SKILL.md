---
name: run-client
description: Runs a client step by step through the Agency Autopilot connector — onboarding, the tracking stack, website fixes, Google Ads, then a weekly cycle — doing one step at a time with every change previewed. Use when the person says "run [client]", "onboard my new client", "take over [client]'s marketing", "what should I do next for [client]", or is new to marketing and wants Agency Autopilot to lead. Works for any client already created on the dashboard or with create_client.
---

# Run a client

For agencies that want Agency Autopilot to lead: it knows the order, checks the client's real accounts, and does the next thing. The person stays in charge of every change.

## The loop
1. `client_playbook` with the client's name. Read back, in plain words: where they stand (`progress`), what is done, and the one `next` step with its `why`.
2. Do that step with the tools in `next.tools`, following `next.do`. Every tool that changes something returns a preview first: show it, and confirm only on the person's clear yes. Never pass `confirm: true` on a first call.
3. Say what changed in one or two sentences, then call `client_playbook` again and offer the next step. Stop whenever the person wants to.

## Onboarding a new client
- Usually the client has already given the agency manager access to their Google Ads, Analytics, Search Console and Business Profile. Then the first step is only linking: `list_connected_accounts`, then `link_account` for each of the client's accounts, and the website address.
- If an account is missing, the client has not added the agency's Google login yet: say which login to invite.
- `client_access_link` is the option when the owner would rather sign in to their own accounts than add the agency as a manager. Offer it; never require it.

## Steps that do not apply
`playbook_mark` with `status: "skipped"` and a short note — for example the client does not run ads (skip `ads_account`, and the whole ads phase is skipped), or tracking was installed by their web developer (`done`). `status: "todo"` undoes a mark.

## Phases, in order
1. Get set up — connect and link accounts · baseline audit · rank tracking
2. Tracking foundations — Analytics property · leads as key events · tags on the site (WordPress, Tag Manager, or snippets for a site built in code) · Google Ads conversion actions · Search Console and sitemap
3. Website — fix what the site check flags
4. Google Ads — the account · a campaign built from keyword research, created paused · cutting wasted spend
5. Every week — rankings, change review, search-term negatives, and a client report that leads with the wins

## Autopilot (Premium)
Once a client is set up, Autopilot can run it hands-off: leads and forms, tracking, SEO, Google Ads and Business Profile, within the limits the agency allowed, on the person's own AI assistant plan. Use the `autopilot` skill: `autopilot_set` (shadow first), `autopilot_start` to run a pass now, `autopilot_routine` for the weekly task, `autopilot_status` for what it did and what needs a yes. It scores its own past changes on leads every run and undoes what hurt.

## When leads stop
`lead_check` for the client whenever the person says leads dropped, the phone went quiet, or "is the form working". It tests every lead signal against its own earlier rate while traffic continues, tells a broken form from a broken tracking tag, loads every page the ads send people to, and names the likely cause and the fix. Lead the answer with what to do today. Clients with Autopilot in shadow or on are checked daily, and `client_overview` carries any open `lead_alerts`: raise those before any other number.

## Rules
- One step at a time. Never batch several steps' changes into one confirmation.
- A step marked `unknown` could not be checked: say so and confirm by hand; never assume it is done.
- New campaigns and ads stay paused until the person explicitly says to turn them on; budgets and ad text are shown verbatim first.
- Write to the client in the owner's own terms when a step ends in a report: their name first, the wins first.
