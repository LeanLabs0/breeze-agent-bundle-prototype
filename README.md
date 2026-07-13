# Breeze Agent Bundle — funnel prototype

Two-page funnel for the HubSpot Breeze agent giveaway. Lean Labs installs free Breeze agents (AEO, SEO, CRO) by hand in a client's HubSpot portal; this is the page that gets them to ask for it.

**Live preview:** https://leanlabs0.github.io/breeze-agent-bundle-prototype/

| Page | File | Preview |
|---|---|---|
| Landing | `landing.html` (also `index.html`) | [/](https://leanlabs0.github.io/breeze-agent-bundle-prototype/) |
| Thank you | `thank-you.html` | [/thank-you.html](https://leanlabs0.github.io/breeze-agent-bundle-prototype/thank-you.html) |

> **The form is the point.** This page exists to get form fills. If nobody submits it, no more agents get built. Everything above the form is there to get someone into it, and the tracking exists to prove whether it worked.

## Running it

No build step, no dependencies. Open the file, or:

```bash
python -m http.server 8899
# http://localhost:8899/landing.html
```

Fonts load from Google Fonts. Artwork is SVG in `assets/`. Everything else is inline.

## Structure

```
landing.html          Landing page. CONFIG block is at the bottom of the file.
thank-you.html        Thank-you page. Partner-ID link + question form.
index.html            Copy of landing.html (so GitHub Pages serves it at /)
assets/logo/          Lean Labs wordmark + icon
assets/agents/        5 x 1:1 agent card images, 1 x 9:16 collage
```

## Before this goes live

Search the code for `<<< FILL ME` and `TODO`. Four things:

1. **HubSpot form** — `HUBSPOT_PORTAL_ID` and `HUBSPOT_FORM_GUID` in the CONFIG block of `landing.html`. Until they're set, the form validates, logs the payload to the console, and redirects — but **saves nothing**. Needs two custom properties in HubSpot: `hubspot_portal_id` and `requested_agents`.
2. **Partner-ID link** — the main button on `thank-you.html` is `href="#"`. This click is the real conversion (a form fill is a lead; this is portal access).
3. **Video** — Kevin records it. Two slots have a commented-out `<iframe>` ready: landing hero and thank-you top.
4. **Question form** — on `thank-you.html`. Currently shows a confirmation without sending anywhere.

## Design rules (from Kevin — please don't undo)

- **Use the regular site nav and footer.** The dashed placeholder boxes mark where they go. *"We're not going to have pages with custom navs and footers that we have to maintain."*
- **Don't add anything to the main nav.** That's why "Get the bundle" is a floating button — appears on scroll, hides at the form.
- **No centered text columns.** Everything is left-aligned on purpose.
- **All agent checkboxes tick by default**, and "Give me the full bundle" re-ticks them.

## Editing the agents

One place — the `AGENTS` array at the bottom of `landing.html`. It drives both the cards and the form checkboxes.

```js
const AGENTS = [
  { name: 'AEO Fan-Out Query Agent',
    img:  'assets/agents/aeo-fan-out.svg',
    line: 'Finds the questions AI asks about your topic before it recommends anyone.' },
  // ...
];
```

Keep descriptions to **one line** — Kevin asked for shorter copy and bigger images.

The agent artwork is first-pass SVG. Kevin is picking the finals; swapping one is a single path change above.

## Tracking

Kevin can't currently see clicks to the page. These events fix that. They fire to HubSpot (`_hsq`), GA4 (`gtag`) and `dataLayer`, each guarded so nothing breaks if one isn't loaded.

| Event | When |
|---|---|
| `breeze_landing_view` | Landing loads |
| `breeze_select_all_agents` | "Give me the full bundle" |
| `breeze_upsell_click` | "See what else we do" |
| `breeze_form_submit` | Form submitted (includes which agents) |
| `breeze_thankyou_view` | Thank-you loads |
| `breeze_partner_connect_click` | **The conversion** — granting portal access |
| `breeze_question_submit` | Question form used |

## Unverified claims ⚠️

Both are live on the page and neither is confirmed:

- **"$100M in client revenue"** — in the H1
- **"70+ five-star reviews"** — under the hero CTA

Confirm or change before this is public.

## Full handoff

See `EDWARD-HANDOFF.md` in the AIS-OS repo (`projects/lean-labs/breeze-agents/`).
