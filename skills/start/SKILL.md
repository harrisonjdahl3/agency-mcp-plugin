---
name: start
description: First-run guide for Agency Autopilot (also called Agency MCP). Use when the user has just installed or connected it, asks how to set it up, what it can do, what to do first, says "set up my first client", "get started", "is it working", "test it", or opens a fresh session with a greeting or a vague first request about marketing, clients or accounts. Calls get_started and explains the value in the agency's own terms, then offers one thing to run right now, and, if no client is set up yet, sets up the first one in the chat.
---

# Start

1. Call `get_started`. It returns what this agency has connected, its clients and what is
   linked to each, a `status`, the single `next_step`, and `try_next` prompts.
2. Answer in under 150 words, in the agency's units (clients, leads, spend, reports), never in
   tool names:
   - One sentence on what this is: their clients' Google, Meta, WordPress and CRM accounts,
     readable and workable from this chat, with every change previewed before it happens.
   - What is ready for them right now, naming their actual clients.
   - The `next_step`, only if `status` is not `ready`. If it is a dashboard step, give the link.
   - If `status` is `no_clients`, do not send them to the dashboard: call `suggest_clients`, name the
     likeliest businesses, ask once what else the business has (Google Ads, Facebook or Instagram ads,
     Business Profile, website and whether it is WordPress, CRM) and connect what is missing, then create
     the one they pick with `create_client` (preview, then confirm) and close anything in its `client_setup`.
   - A business they do not manage yet (a pitch): the `prospect-audit` skill — `audit_prospect` works
     with nothing connected; `create_prospect_report` publishes the page to send.
   - New to marketing, or wants Agency Autopilot to lead a client end to end: the `run-client` skill (`client_playbook`) — onboarding, tracking, website, ads and the weekly cycle, one previewed step at a time.
   - Where a client ranks, or rankings for a report: the `rank-tracking` skill (`rank_track`, `rank_report`).
   - A site the agency built in code: the `site-tracking-in-code` skill (ids and snippets from
     `tracking_spec`, edits in the codebase, `site_check` to verify). Never edit a codebase through
     the connector; it has no tool for that.
   - If a tool says an account "is not linked to any of your clients" and a client exists, use
     `link_account` (preview, then confirm) rather than sending them to the dashboard.
   - If a platform they want is not connected, or they want a second login or a link for a client
     to connect their own accounts, use the `connect-accounts` skill (`connect_platform`,
     `client_access_link`). Never ask for a token or password in the conversation.
   - If they ask what is new, read `whats_new` and name the two or three most useful additions.
3. End by offering to run the first `try_next` item immediately, as a question. If no client is set up
   yet, the offer is always the same one: "Want me to set up your first client now? It takes about a
   minute." Most people who connect never take a first step (10/20/26: 63% of sign-ups in two weeks),
   so make this one obvious and easy.

If they ask "what can you do for <client>?", call `get_started` if you have not already, then
answer for THAT client only: which channels are linked, three things you can do with them
(a report, a diagnosis, a change that would be previewed first), and offer to do the first one.
