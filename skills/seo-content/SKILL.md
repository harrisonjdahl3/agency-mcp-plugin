---
name: seo-content
description: Drafts or optimises a page or blog post on a client's website — WordPress through the Agency Autopilot connector, or any other builder through the assistant's own connector for it — including the SEO title, meta description and JSON-LD schema. Use when asked to write a service page, landing page, location page, blog post, or to "optimise this page", "fix the title tags", "add schema", "rewrite the meta descriptions". Creates drafts and previews every change; publishes only when the person explicitly says to publish.
---

# SEO content on a client's website

## Before writing
1. `list_clients` → the client's `wordpress_site` label.
   - **No WordPress link here?** Check your own tools for a connector to this site or its builder (Webflow, Wix, WordPress.com, a WordPress plugin connector). If one exists, write the content with this skill's rules and make the edit through that connector: show the exact change first, apply it only on the person's yes, then `site_check` the URL to confirm it is live. If none exists, deliver the finished copy, SEO title and description for the person to paste, and mention that connecting the site (dashboard → Websites) or adding their builder's own connector lets you make the edit next time.
   - **Connected both here and through another connector?** Pick one and use it for the whole job.
   The steps below are the WordPress-through-Agency-MCP path.
2. `wp_site_capabilities` once per site: confirms the bridge plugin, so SEO fields and schema can be set.
3. Take the target from the client's keyword map (`keyword_strategy`, action get): the cluster this page should win, its searches and their volumes. No map yet? Build one (action research, then save) or, for a one-off, use the `keyword-research` brief, or ask for the primary keyword and the page's job (rank for X, convert visitors to calls or form fills).
4. Study the pages that win before you write: `competitor_pages` with the service (add a comparable market with strong winners, e.g. "Fresno, CA" for a shed builder, when the local ones are weak). Build from the pattern they share: the sections they all cover, their length, where they put the form and phone, the schema they use. Mirror structure and coverage, never their words or a claim the client hasn't made.
5. For an existing page: `wp_get_post` first. Read what's there; keep what works. Check `site_health` for that page (speed, a cut-off sentence, missing H1, indexing) and fix those too.

## Writing standard (local service business)
- One page, one primary keyword, in the title, H1, first paragraph and URL slug.
- Structure: what we do → where (city or service area) → why us (proof, years, guarantees) → how it works → FAQ (3–5 real questions) → call to action with phone number and form.
- Plain language a homeowner uses. Short sentences. No filler adjectives.
- Internal links: to the two or three most related service pages and the contact page.
- Length follows the job: service pages 600–1,000 words, location pages 400–700, posts as long as the answer needs.

## Written to be quoted by AI answers too (ChatGPT, Perplexity, Claude, Google's AI Overviews)
- Answer first: the opening paragraph says what the business does, where and for whom, in plain words, before any story. AI answers and Google's snippets quote the first lines.
- Real numbers only: a price range, a permit rule, a timeline, the CCB or licence number, years in business, all from the client's own facts or attributed sources. Never invent a statistic or a claim.
- Questions as questions: the FAQ uses the customer's own question as a visible heading, with a two or three sentence answer right under it (and FAQPage schema that mirrors only what's visible).
- Name things exactly: the town, the material, the service ("cedar privacy fence in Keizer"), not "our solutions".
- `site_check` reports `ai_search`: AI search crawlers blocked in robots.txt (a real problem), whether the home page's opening and FAQ give an answer to quote, and `/llms.txt` (optional; few systems read it yet).

## Apply, always as a preview first
- New page: `wp_write_post` with `type: "page"`, `title`, `slug`, `content` (clean HTML: h2/h3, p, ul, a). Omit `status` so it is created as a **draft**.
- Existing page: `wp_write_post` with `id`, changed fields only. It previews; call again with `confirm: true` only after the person agrees.
- SEO fields: `wp_set_seo` with `title` (50–60 chars), `description` (140–160 chars), `canonical` if needed, and `schema` as a JSON-LD string — `LocalBusiness` (or a subtype like `HomeAndConstructionBusiness`) for service pages, `FAQPage` when there is an FAQ. Preview, then `confirm: true`.
- Text on an existing page: `wp_replace_in_post`. Its preview reads the live page: if it says the edit would change nothing visitors see (Elementor content the theme doesn't render, or text that only sits in a fallback copy), don't make it; change that text in wp-admin or tell the person exactly what to change. After the confirmed call, `live` says whether the new text is on the live page; it isn't done until it is.
- Showing a change before making it ("show me first"): build the preview from the live page's own HTML, with its real header, logo, fonts, CSS and footer and a `<base href>` to the site. Change only the content inside its real sections, strip analytics and ad tags, label it DRAFT, and say any embedded form is live. Never invent colours, fonts or a logo; if the live page can't be read, say so instead of mocking it up.
- Images: `wp_add_image` from a URL, with alt text naming the service and place.
- Publishing: only with `status: "publish"` and `confirm: true`, and only when the person said "publish".

## After it goes live
`site_health` with `refresh: true` and the page's `url`: confirm the visitor view shows the change (a cached copy says so), the form is still there, and Google can index it. A title change is checked on the live page by `wp_set_seo` itself; if it says the SEO plugin owns the title and the Bridge is old, the agency updates the Bridge (1.4.1+) once.

## Output
Show the draft or the diff, the SEO title and description with character counts, the schema type, and the exact confirmation you need. Then the link to the draft.

## Rules
- Never publish, never overwrite an existing page, without the explicit word.
- Never claim reviews, awards or years in business that were not given to you.
- Never approximate a client's site in a preview: it is built from the live page or not shown.
- If the site check fails (`wp_check_site`), report the reason and stop; do not queue changes blind.
