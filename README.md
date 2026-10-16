<p align="center"><img src=".claude-plugin/icon.png" alt="Agency Autopilot logo" width="96" height="96"></p>

# Agency Autopilot plugin for Claude Code

_Agency Autopilot was called Agency MCP until October 2026. The connector address, plugin id (`agency-mcp`) and install commands are unchanged._

Also works as a connector in ChatGPT, Codex and any assistant that takes a custom MCP server: add `https://mcp.jiyumarketing.com/mcp` and sign in. The skills below are written for any assistant; only the install commands are Claude Code's.

Run a marketing agency from the conversation. This plugin installs the **Agency Autopilot** connector and a set of skills that know how to use it: **Autopilot** (a hands-off weekly pass that works each client end to end and learns from what brings in leads), SEO audits, keyword research, on-page content, Google Ads and Meta Ads management, hosted lead forms, CRM follow-up and branded client reports.

Agency Autopilot (https://getagencyautopilot.com) connects the accounts your agency already manages — Google Analytics, Search Console, Google Ads, Meta ads, WordPress and HighLevel — to Claude. It only ever sees what your own logins can reach, and **every change is previewed first and applied only when you confirm it.**

## Install

```
/plugin marketplace add harrisonjdahl3/agency-mcp-plugin
/plugin install agency-mcp@agency-mcp
```

Then type **"How do I set Agency Autopilot up?"** — it checks what is connected, names your clients, and tells you the one next step.

The first tool call opens a Google sign-in for the Agency Autopilot connector. Sign in with the Google account you use to manage your clients. Then link each client's accounts once at https://mcp.jiyumarketing.com/dashboard → Clients, so reports and per-client questions resolve by name.

Pricing: free for one client; paid plans are per client (see the site). No cost is charged through this plugin.

## Skills

| Skill | What it does |
|---|---|
| `autopilot` | Runs Autopilot on a client: leads and forms, tracking, the website and SEO, Google Ads and Business Profile, within the limits you set, on your own Claude plan. Starts in shadow; scores its own changes on leads and undoes what hurt; sets up the weekly task. Premium. |
| `start` | First-run guide: what is connected, what to do next, and what to ask first. Triggers on "how do I set this up" / "what can you do". |
| `connect-accounts` | Connects a platform or an extra login from the conversation with a one-time link, creates a link a client can use to connect their own accounts instead of granting manager access, and groups accounts into clients with a name check so one business's data never lands under another. |
| `client-audit` | A scorecard of what to fix first across Google Ads, Meta ads, Search Console and Analytics, with the amount each problem is costing. "Do it all" then previews the keyword pauses and negative keywords together and applies them in one confirmed step. |
| `weekly-review` | Every change you confirmed, with the week before set against the week after: win, loss, flat or too new to judge. |
| `seo-audit` | Site-level SEO health from Search Console, Analytics and the live WordPress site: what ranks, what's slipping, what to fix first. Also sets Search Console up for a new site: add it for a client, verify ownership, submit the sitemap. |
| `keyword-research` | Keyword volumes, competition and bid ranges from Google Ads planning data, joined with what the site already ranks for. |
| `seo-content` | Drafts or optimizes a page or post on the client's WordPress site (Elementor pages included, with the Bridge plugin 1.3.0+), including title, meta description and JSON-LD. |
| `google-ads-manager` | Reports, search-term hygiene, negatives, budgets, pauses and new campaign builds, all previewed and validated by Google before anything applies. |
| `meta-ads-manager` | Meta (Facebook and Instagram) campaign reporting, status and budget changes with the same preview-then-confirm discipline. |
| `find-prospects` | Finds local businesses in any trade and area, checks each website, ranks the ones you could help most and drafts a first email, follow-up and phone opener. One credit per business; 100 free to start. Nothing is sent. |
| `crm-followup` | HighLevel: who is waiting on a reply, pipeline and won-work numbers, appointments, and prospect creation when writes are enabled. |
| `client-report` | A clear, jargon-free weekly or monthly report across every channel a client has linked, published as a branded shareable page. |
| `local-seo-gbp` | Google Business Profile reviews, posts and performance — activates when the Business Profile tools are enabled on your account. |

## How the skills behave

- They start from `list_clients` and `list_connected_accounts`, never from remembered IDs.
- Reads are free-form. Writes (a campaign change, a post, a reply, a message) always run as a preview first and are applied only on your explicit confirmation.
- Numbers always come with the date range they cover.
- If a client's account isn't linked yet, the skill sets the client up in the conversation rather than guessing.
- If a platform isn't connected yet, the skill gives you a one-time link to connect it. Tokens and passwords are entered on that page, never in the conversation.

## Support

harrison@jiyumarketing.com · Privacy: https://mcp.jiyumarketing.com/privacy · Terms: https://mcp.jiyumarketing.com/terms

Not affiliated with, sponsored by, or endorsed by Google, Meta, HighLevel, Automattic or Anthropic.

## Rankings
- `run-client` — runs a client step by step: onboarding, tracking stack, website fixes, Google Ads and the weekly cycle (`client_playbook`, `playbook_mark`), then hands it to Autopilot.
- `rank-tracking` — where a client ranks: Search Console position for up to ten terms daily and the map pack weekly (`rank_track`, `rank_report`).

## Gemini CLI

```
gemini extensions install https://github.com/harrisonjdahl3/agency-mcp-plugin
```

Then run `/mcp auth agency-mcp` inside Gemini and sign in. The same skills load from `skills/`.
