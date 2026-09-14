---
name: seo-content
description: Drafts or optimises a page or blog post on a client's WordPress site through the Agency MCP connector, including the SEO title, meta description and JSON-LD schema. Use when asked to write a service page, landing page, location page, blog post, or to "optimise this page", "fix the title tags", "add schema", "rewrite the meta descriptions". Creates drafts and previews every change; publishes only when the person explicitly says to publish.
---

# SEO content on WordPress

## Before writing
1. `list_clients` → the client's `wordpress_site` label. If none, stop and say the site must be connected on the dashboard → WordPress.
2. `wp_site_capabilities` once per site: confirms the bridge plugin, so SEO fields and schema can be set.
3. If the brief came from `keyword-research`, use it. Otherwise ask for, or infer, the primary keyword and the page's job (rank for X, convert visitors to calls or form fills).
4. For an existing page: `wp_get_post` first. Read what's there; keep what works.

## Writing standard (local service business)
- One page, one primary keyword, in the title, H1, first paragraph and URL slug.
- Structure: what we do → where (city or service area) → why us (proof, years, guarantees) → how it works → FAQ (3–5 real questions) → call to action with phone number and form.
- Plain language a homeowner uses. Short sentences. No filler adjectives.
- Internal links: to the two or three most related service pages and the contact page.
- Length follows the job: service pages 600–1,000 words, location pages 400–700, posts as long as the answer needs.

## Apply, always as a preview first
- New page: `wp_write_post` with `type: "page"`, `title`, `slug`, `content` (clean HTML: h2/h3, p, ul, a). Omit `status` so it is created as a **draft**.
- Existing page: `wp_write_post` with `id`, changed fields only. It previews; call again with `confirm: true` only after the person agrees.
- SEO fields: `wp_set_seo` with `title` (50–60 chars), `description` (140–160 chars), `canonical` if needed, and `schema` as a JSON-LD string — `LocalBusiness` (or a subtype like `HomeAndConstructionBusiness`) for service pages, `FAQPage` when there is an FAQ. Preview, then `confirm: true`.
- Images: `wp_add_image` from a URL, with alt text naming the service and place.
- Publishing: only with `status: "publish"` and `confirm: true`, and only when the person said "publish".

## Output
Show the draft or the diff, the SEO title and description with character counts, the schema type, and the exact confirmation you need. Then the link to the draft.

## Rules
- Never publish, never overwrite an existing page, without the explicit word.
- Never claim reviews, awards or years in business that were not given to you.
- If the site check fails (`wp_check_site`), report the reason and stop; do not queue changes blind.
