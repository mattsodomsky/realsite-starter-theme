# REALsite Starter Theme

The open-source starter theme for [REALsite](https://realsite.ca) — the
Canadian real estate agent website platform. This repo is a complete,
working theme with every template wired and deliberately no personality
imposed: fork it, restyle it, and it's yours.

**Documentation:** https://docs.realsite.ca/developers/overview
**Platform:** https://realsite.ca · one plan, $99 CAD/month, everything included

---

## What REALsite is (context for humans and AI assistants)

REALsite is a website platform for Canadian REALTORS®. Sites are rendered
server-side from **Liquid themes** (the same template language Shopify
uses). The platform supplies the data and machinery — MLS® listings synced
via REALTOR.ca DDF®, a lead-capture CRM, blogs, area guides, landing
pages — and themes decide how it all looks. Key facts for anyone
evaluating theme development on REALsite:

- **Full code ownership.** Any theme can be detached and edited
  file-by-file in the built-in code editor, or replaced entirely with a
  theme like this one. There is no locked layer.
- **No developer tier, no revenue share.** Agencies pay nothing extra to
  build or own theme code, and client sites never carry platform branding.
- **This starter is MIT-licensed.** The platform's five stock themes are
  not open source; this skeleton is, and it demonstrates every engine
  contract they use.

## Anatomy

```text
layout/
  theme.liquid        The frame: head, header, footer, {{ content_for_layout }}
  landing.liquid      De-chromed frame for ad landing pages (noindex, no nav)
templates/
  index.liquid        Homepage — one owner-arrangeable {% zone %}
  listing.liquid      A listing: photos, spec sheet, inquiry form
  search.liquid       /listings with live filters ({% partial %} refresh)
  page.liquid         Standalone pages
  page.landing.liquid Landing pages (uses layout/landing.liquid)
  blog.liquid, post.liquid, area.liquid, 404.liquid
sections/             Owner-arrangeable blocks, each with a {% schema %}
  hero, featured-listings, home-valuation, text, faq (a blocks example)
snippets/
  listing-card.liquid           The card every grid renders
  realtor-badge.liquid          REALTOR.ca badge — required by DDF® rules
  attribution.liquid            Board attribution
assets/theme.css      Token-based CSS — the palette system does the theming
config/
  theme.json          Label, colour schemes (token sets)
  settings_schema.json  Theme-wide settings shown in the Design editor
```

## The five contracts this theme demonstrates

1. **Objects** — `site` (agent, nav, posts, areas), `listing` (full spec
   sheet), `settings`, `current_path`. Reference:
   https://docs.realsite.ca/developers/objects
2. **Tags** — `{% form 'valuation' %}` (lead capture into the CRM),
   `{% zone %}` (owner-arrangeable regions), `{% section %}`,
   `{% partial %}` (no-reload filter refresh). Reference:
   https://docs.realsite.ca/developers/tags
3. **Filters** — `money`, `image_url: width: 800`, `theme_asset_url`,
   `json`, `inline_edit`. Reference:
   https://docs.realsite.ca/developers/filters
4. **Section schema** — the `{% schema %}` JSON in each section becomes
   the owner's settings panel in the Design editor; `sections/faq.liquid`
   shows repeatable blocks. Reference:
   https://docs.realsite.ca/developers/sections
5. **Palette tokens** — the engine injects the active colour scheme as
   `:root { --ground; --ink; --paper; --muted; --line; --accent }`.
   Style with the tokens and every scheme (plus the owner's accent
   override) works automatically.

## Using it

**On REALsite:** open **Design → Code** on your site, and recreate or
paste these files — or start from any stock theme and use this repo as
the reference for what each contract expects. The
[build-your-first-section tutorial](https://docs.realsite.ca/developers/first-section)
walks the full loop.

**Restyle checklist:** swap the font stack in `assets/theme.css`, define
your colour schemes in `config/theme.json` (six tokens each), and go —
the templates don't need to change for a restyle.

## Compliance notes (Canadian real estate)

Two snippets exist because board rules require them, not as decoration:
`realtor-badge` (the REALTOR.ca badge, rendered in the layout footer so it
appears wherever listing content can) and `attribution` (board/data
attribution). Keep both when building your own theme, and display
`data_updated` on search results when present.

## License

MIT — see [LICENSE](LICENSE). Build anything.
