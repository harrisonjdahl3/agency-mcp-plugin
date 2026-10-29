---
name: sign-clients
description: Drafts, previews and sends a client agreement through the Agency Autopilot connector, then gets the client signed and paid into the agency's own Stripe. Use when the person says "draft an agreement for [business]", "send them a contract", "sign this client", "they said yes", "what do I charge them", or after a prospect is interested. Works on every plan. The agency's offer (services and usual prices) is asked once and reused.
---

# Sign clients

From "they said yes" to a signed, paying client without leaving the chat. The agreement fills itself in from what Agency Autopilot already knows (the agency's prospects, clients and CRM, and its branding), the person sees a live preview, and the client signs on their phone and pays straight into the agency's own Stripe account.

## The flow
1. **The offer, once.** If the agency hasn't saved what it sells and its usual prices, ask once ("What do you usually sell, and at what price?") and save it with `agreement_offer_save`. After that it is the default for every agreement; any price the person names wins.
2. **Draft.** `agreement_draft` with `client` as the person said it (a prospect from a search, an existing client, or a CRM contact), the services and the money (monthly, one-time or setup, start date, term). Never guess a price: if none was named and no offer is saved, ask. The result lists anything still missing (usually just the contact's email); ask only for that. Never invent a person's name or email.
3. **Show the live preview.** Give the preview link. It shows the finished agreement in the agency's branding and updates itself when the draft changes.
4. **Change by talking.** "Make it $1,200", "add Google Ads", "start on the 1st": `agreement_update` on the same draft.
5. **Send.** `agreement_send` previews first; send only on the person's yes (or they press Send on the preview page while signed in).
6. **Follow it.** `agreement_status` shows opened, signed and paid. The agency is emailed when the client opens, signs, pays, or a payment fails.
7. **Paid → set the client up.** Signing creates the client in Agency Autopilot. Once paid, offer the next step: `client_access_link` so the client connects their own accounts, then Autopilot's set-up for that client.

## Stripe
Payments need the agency's Stripe connected once (the dashboard's Agreements tab, Connect Stripe). Without it, the agreement can still be drafted, previewed, sent and signed.

## Rules
- The wording is a general standard agreement the agency can change ("Customize wording" on the Agreements tab). It is not legal advice; never say it is reviewed by a lawyer.
- `agreement_void` cancels an unsigned agreement (preview first).
- Never mention fees or internal ids to the person or the client.
