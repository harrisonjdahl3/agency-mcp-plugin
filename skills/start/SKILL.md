---
name: start
description: First-run guide for Agency MCP. Use when the user asks how to set Agency MCP up, what it can do, what to do first, or opens a fresh session with it — "how do I set this up", "what can you do", "where do I start". Calls get_started and explains the value in the agency's own terms, then offers one thing to run right now.
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
3. End by offering to run the first `try_next` item immediately, as a question.

If they ask "what can you do for <client>?", call `get_started` if you have not already, then
answer for THAT client only: which channels are linked, three things you can do with them
(a report, a diagnosis, a change that would be previewed first), and offer to do the first one.
