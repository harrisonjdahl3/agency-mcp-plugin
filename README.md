# Agency MCP plugin for Claude Code

Run a marketing agency from the conversation. This plugin installs the **Agency MCP** connector and a set of skills that know how to use it: SEO audits, keyword research, on-page content, Google Ads and Meta Ads management, CRM follow-up and branded client reports.

Agency MCP (https://mcp.jiyumarketing.com) connects the accounts your agency already manages — Google Analytics, Search Console, Google Ads, Meta ads, WordPress and HighLevel — to Claude. It only ever sees what your own logins can reach, and **every change is previewed first and applied only when you confirm it.**

## Install

```
/plugin marketplace add harrisonjdahl3/agency-mcp-plugin
/plugin install agency-mcp@agency-mcp
```

The first tool call opens a Google sign-in for the Agency MCP connector. Sign in with the Google account you use to manage your clients. Then link each client's accounts once at https://mcp.jiyumarketing.com/dashboard → Clients, so reports and per-client questions resolve by name.

Pricing: free for one client; paid plans are per client (see the site). No cost is charged through this plugin.

## Skills

| Skill | What it does |
|---|---|
| `seo-audit` | Site-level SEO health from Search Console, Analytics and the live WordPress site: what ranks, what's slipping, what to fix first. |
| `keyword-research` | Keyword volumes, competition and bid ranges from Google Ads planning data, joined with what the site already ranks for. |
| `seo-content` | Drafts or optimizes a page or post on the client's WordPress site, including title, meta description and JSON-LD, as a draft until approved. |
| `google-ads-manager` | Reports, search-term hygiene, negatives, budgets, pauses and new campaign builds, all previewed and validated by Google before anything applies. |
| `meta-ads-manager` | Meta (Facebook and Instagram) campaign reporting, status and budget changes with the same preview-then-confirm discipline. |
| `crm-followup` | HighLevel: who is waiting on a reply, pipeline and won-work numbers, appointments, and prospect creation when writes are enabled. |
| `client-report` | A plain-English weekly or monthly report across every channel a client has linked, published as a branded shareable page. |
| `local-seo-gbp` | Google Business Profile reviews, posts and performance — activates when the Business Profile tools are enabled on your account. |

## How the skills behave

- They start from `list_clients` and `list_connected_accounts`, never from remembered IDs.
- Reads are free-form. Writes (a campaign change, a post, a reply, a message) always run as a preview first and are applied only on your explicit confirmation.
- Numbers always come with the date range they cover.
- If a client's account isn't linked yet, the skill tells you to link it on the dashboard rather than guessing.

## Support

harrison@jiyumarketing.com · Privacy: https://mcp.jiyumarketing.com/privacy · Terms: https://mcp.jiyumarketing.com/terms

Not affiliated with, sponsored by, or endorsed by Google, Meta, HighLevel, Automattic or Anthropic.
