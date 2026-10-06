---
name: connect-accounts
description: Connects platforms and logins to an Agency MCP account and groups the accounts into clients without mixing businesses up. Use when a tool says a platform is not connected, when the user wants to connect Google, Meta ads, Business Profile, the CRM or a WordPress site, add a second login for a platform, let one of their clients connect their own accounts instead of granting manager access, or set up or fix which accounts belong to which client — "connect my Meta ads", "add another Google account", "send Acme a link to connect their accounts", "set up a client", "this client is showing the wrong data".
---

# Connect accounts and group them into clients

## Connecting a platform
`connect_platform` with `platform`: `google`, `meta`, `business_profile`, `crm` or `wordpress`. It returns a one-time `link` that lasts 30 minutes.
- Give the person the link as a clickable link. Google, Meta and Business Profile go to that platform's own sign-in. The CRM and WordPress open a short form on Agency MCP's site.
- Never ask for a token, an application password or any other secret in the conversation. The person enters it on the page the link opens.
- When they say it is done, run the tool that was refused again.
- If a tool reports "not connected", offer the link once. Do not retry the tool until they say they have connected.

## More than one login on a platform
An account can hold several Google logins, several Facebook logins, several Business Profile logins, one CRM token per client sub-account, and any number of WordPress sites.
- `connect_platform` adds a login; it does not replace the existing one. For a different Google or Facebook user, the person must choose that other account on the sign-in screen (logging out of Facebook first, or using a private window, makes Facebook ask).
- List tools return everything every login reaches. With several logins each row carries `google_login` or `meta_login`: two accounts can share a name, so say the login when you name the account.
- Each account is reached through the login that has access to it. Nothing needs to be chosen per request.

## When a client will not give manager access
`client_access_link` with `client` (their name) and `platforms`. It returns a 14-day link the agency sends to that client.
- The link is for the client, not the agency. The page names the agency, says what each connection allows and how to undo it, and the client signs in with their own logins.
- Tell the person to send it themselves, by email or text.
- When the client has connected, their accounts appear in the list tools marked with their login. Then link them to the client as below.

## Grouping accounts into a client
A client is one business. Connecting a login links nothing by itself.
1. `suggest_clients` groups unlinked accounts by business name. `list_clients` shows what exists.
2. `create_client` with the ids exactly as the list tools returned them. Call it without `confirm` first.
3. The preview's `accounts` list shows each account by NAME with a `check`: `match`, `possible`, `unknown` (the name says nothing) or `different`. Show the person the names, and the login for each when there are several logins.
4. If the preview has `warnings`, stop. An account that looks like a different business, or accounts that match each other but not the client's name, mean the wrong data would be attached. Ask about each named account. Set `acknowledge_mismatch: true` only after the person has said that named account belongs to this client.
5. On a clear yes, call again with `confirm: true`.

Why it matters: a wrong link is silent. Every report, audit and ad change for that client would then be about another business.

## Adding an account to a client that already exists
This is the usual case after connecting a platform: the client is set up, the new account is not placed yet, and a tool refuses with "not linked to any of your clients".
1. `link_account` with the client's name exactly as `list_clients` returns it and the ids as the list tools returned them. Call it without `confirm` first.
2. The preview has the same `accounts` list and `warnings` as `create_client`; the same rules apply. Show the names, ask about anything flagged, and set `acknowledge_mismatch: true` only after the person has confirmed that named account.
3. If the preview has `replaces`, the client already has a different account of that kind. Name both to the person; call again with `replace: true` only if they want the swap.
4. On a clear yes, call again with `confirm: true`, then re-run the request that was refused.

Links can also be edited on the dashboard (Clients, then Edit on the client), where a client card shows an amber note when an account does not look like that client.

## Rules
- Ids come from the list tools, never from memory or guesswork.
- One account belongs to one client. If `create_client` or `link_account` says an account is already linked to another client, report that; do not look for a way around it.
- Turning CRM changes on, billing and removing a login are done on the dashboard.
