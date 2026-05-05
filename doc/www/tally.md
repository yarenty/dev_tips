---
title: Tally — free form & survey builder
main_link: https://tally.so/
keywords: [tally, forms, surveys, no-code, saas, typeform-alternative]
status: reviewed
---

# Tally — free form & survey builder

**Main link:** <https://tally.so/>

## Summary

**Tally** is a free, intuitive form builder — Typeform-style conversational forms but with a far more generous free tier (unlimited forms, unlimited responses, 999+ submissions on free). Drag-and-drop fields, conditional logic, calculations, payments via Stripe, hidden fields, file uploads, and integrations with most automation platforms (n8n, Zapier, Make, Airtable, Notion). Used in this vault as the lightweight "free form" answer when you need to collect data from non-tech humans.

## Insight

Use it as the **default free option** for any "I need a form for users to fill in". The free tier is genuinely usable for personal projects, small businesses, and one-off events; the paid tiers are reasonable when you need branding, custom domains, or higher submission limits. Pairs naturally with [[airtable|Airtable]] (form submissions → Airtable rows) and [[../ml/tools/n8n|n8n]] (form submissions → automation workflows).

When **not** to reach for Tally:
- You need server-side validation / custom backend logic → roll your own with [[loco_rust_on_rails|Loco]] or any web framework.
- You need complex conditional flows with branching scoring → SurveyMonkey, Qualtrics.
- You're in healthcare / banking / regulated → use HIPAA/PCI-compliant tools (Tally is not).

## Similar / related topics

- [[airtable]] — Often used together: Tally form → Airtable database.
- **Typeform** — the higher-polish, paid-tier predecessor; Tally is the free reaction to it.
- **Google Forms** — even more free, less feature-rich, more "Google" in the UI.
- **Formspree / Netlify Forms** — for static sites; collect submissions without your own backend.
- [[../ml/tools/n8n|n8n]] — workflow automation for processing submissions.

## Internal links

<!-- reviewed -->

- [[airtable]] — Often paired (Tally → Airtable).
- [[../ml/tools/n8n|n8n]] — Workflow trigger for processing submissions.

## Keywords

`#tally` `#forms` `#surveys` `#no-code` `#saas` `#typeform-alternative`

## References / raw notes

- Project site: <https://tally.so/>
- Pricing: <https://tally.so/pricing>
- Templates gallery: <https://tally.so/templates>

Project pitch: *"Say goodbye to boring forms. Meet Tally — the free, intuitive form builder you've been looking for."*

### Typical integration shape

```
Tally form submission
        │
        ▼
   [Webhook]
        │
        ├──► n8n / Zapier / Make → workflow
        ├──► Airtable / Notion / Google Sheets → row
        └──► Slack / email → notification
```
