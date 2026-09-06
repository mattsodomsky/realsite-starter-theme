# REALsite Starter Theme

The open-source starter theme for [REALsite](https://realsite.ca) — the
Canadian real estate agent website platform. This repo is a complete,
working theme with every template wired and deliberately no personality
imposed: fork it, restyle it, and it's yours.

**Documentation:** https://docs.realsite.ca/developers/overview
**Source:** https://github.com/mattsodomsky/realsite-starter-theme
**Platform:** https://realsite.ca

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
- **This starter is MIT-licensed.** The platform's stock themes are
  not open source; this skeleton is, and it demonstrates the core rendering contracts.

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
  agent-masthead, agent-letter  Plain agent introductions; shared setting IDs
  agent-approach, agent-invitation  Editable service steps and contact invitation
  hero                Legacy heading/subheading hero, retained for saved sites
  about-agent, featured-listings, recent-sales, home-valuation, text, faq
snippets/
  listing-card.liquid           The card every grid renders
  realtor-badge.liquid          REALTOR.ca badge — required by DDF® rules
  attribution.liquid            Board attribution
  agent-introduction.liquid     Shared markup for both agent opening schemas
  contact-dialog.liquid         Contact controls + platform lead-form hooks
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

## Keeping content portable

The agent is the brand. The default homepage leads with `site.agent_name`,
`site.headshot_url`, `site.tagline` and `site.about`. These are site content,
not hard-coded demo names, credentials or reviews. All homepage defaults
are included in this repository; a detached custom theme needs its own files.

Keep the `home-main` zone name when restyling. Existing sites retain their
saved section order, settings and blocks when changing themes; changing a
zone's `default:` list does **not** rewrite an existing arrangement. The
`agent-masthead` and `agent-letter` sections retain the platform's setting
IDs and types, so an existing introduction renders here without renaming
its saved values. `hero` keeps its original `heading` and `subheading` settings.

A stock filesystem theme can fall back to platform shared sections. A
**detached/custom theme does not**: copy every section and snippet referenced
by its defaults or saved arrangements into that theme. If migrating a site
with other section types, retain those files and their schema IDs too;
unsupported section types may be omitted from rendering even though their
saved data remains. Do not promise that arbitrary custom sections transfer
to a theme that does not implement them. Preview the same site's content
before switching, including a round trip back to its original theme.

## Runtime and accessibility contracts

- `data-contact-open`, `data-contact-close` and `data-contact-modal` connect
  buttons to the dialog through the platform's injected `storefront.js`.
  Keep `data-lead-form="contact-modal"` and the matching `sf` hidden field;
  they identify which form should show a success/error message after redirect.
- `{% form %}` supplies ordinary lead forms. The hand-written modal also
  includes its honeypot and `{% marketing_consent %}`. The platform controls
  consent text and signed tokens; do not replace these with a custom checkbox.
- Keep visible labels, required email inputs, the skip link and focus styles.
  Both layouts include the dialog, since landing-page zones can use the same
  contact sections. The runtime is injected by REALsite, not a file to copy
  from a third-party CDN. Optional direct phone/email links remain useful.
- `site.areas` is an array of **names**, not objects. Use `site.area_links`
  when you need each area's name and URL. The valuation example uses names.
- Search pagination uses `prev_url` and `next_url` supplied by the controller;
  building links from only `page` loses the visitor's selected filters.
- Escape text and attribute values; apply `safe_url` to customer-provided
  `href`/`src` values. `inline_edit` emits escaped text and platform-owned
  editing markers. Do not hard-code HTML into ordinary text settings.

## Verification

In the REALsite application checkout, run:

```sh
RAILS_ENV=test bin/rails test test/models/starter_theme_test.rb
```

These tests parse every Liquid file and render the starter through the real
engine, including its contact hooks, saved hero settings, area selector and
filtered pagination. This standalone repository is theme source, not a
Rails app or a static HTML site: it cannot render its platform objects by
opening Liquid files directly in a browser.

## Compliance notes (Canadian real estate)

Two snippets exist because board rules require them, not as decoration:
`realtor-badge` (the REALTOR.ca badge, rendered in the layout footer so it
appears wherever listing content can) and `attribution` (board/data
attribution). Keep both when building your own theme, and display
`data_updated` on search results when present.

## License

MIT — see [LICENSE](LICENSE). Build anything.
