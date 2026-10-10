# Agency MCP

Agency MCP connects the client accounts a marketing agency already manages — Google Ads, Google Analytics 4,
Search Console, Business Profile, Meta ads, the client's website (WordPress or any other) and the agency's CRM —
so client work gets done from this conversation instead of across a dozen consoles.

## First run
1. Run `/mcp auth agency-mcp` and sign in with the Google account the agency already manages clients with.
   A browser window opens; nothing else needs installing.
2. Ask "How do I set Agency MCP up?" — it checks what is connected, names the clients, and gives the one next step.

## How to work with it
- Start from the client: `list_clients` gives the exact names; every other tool takes the client's name or an account
  linked to that client. Accounts not linked to a client are refused on purpose.
- **Every change is previewed first.** Tools that change an account return a preview; apply it only after the person
  agrees, by calling again with `confirm: true`. New campaigns and ads are created paused. Never pass `confirm: true`
  on a first call.
- Reports and audits are reads: `client_overview`, `audit_client`, `site_check` (any website, by URL), `rank_report`,
  `review_changes`. A business the agency does not manage yet: `audit_prospect` with just its website.
- Money is in the account's currency; Google Ads `*_micros` fields are millionths — divide by 1,000,000.
- Say which date range every number covers.

## Skills
The `skills/` folder holds step-by-step workflows (client audit, weekly change review, SEO audit, keyword research,
Google Ads and Meta ads management, CRM follow-up, client reports, prospect audits, rank tracking). Use the one that
matches the request.

Docs: https://mcp.jiyumarketing.com/docs · Free for one client.
