---
title: Web (WWW)
keywords: [web, http, https, web-server, ssg, no-code, profile]
status: reviewed
---

# Web (WWW)

Notes on the **web platform** end of the stack — running web servers, getting HTTPS, generating static sites, building Rails-style web apps in Rust, code-signing Apache modules on macOS, and a couple of low-code / form-builder tools that live naturally in this section.

## Articles

### Servers & TLS
- [Apache HTTPD on macOS — quick commands](apache.md) — `apachectl` cheat-sheet for the macOS-system Apache.
- [HTTPS for websites — Certbot + Apache](ssl.md) — Let's Encrypt + Certbot recipe.
- [Signing 3rd-party Apache modules on macOS Big Sur+](signing_mac_libs.md) — Self-signed CA + `codesign` so `mod_passenger` / `mod_php` load.

### Building sites & apps
- [Static Site Generators (Zola, Tera)](ssg.md) — Single-binary Rust SSG; templates via Tera.
- [Loco — Rails-style web framework for Rust](loco_rust_on_rails.md) — Convention-over-configuration full-stack alternative.
- [fluent-templates — Fluent i18n for Rust templates](fluent_templates.md) — Mozilla's Fluent + Tera/Handlebars wiring.

### No-code & forms
- [Tally — free form & survey builder](tally.md) — Free Typeform-style form builder; pairs with Airtable + n8n.
- [Airtable — spreadsheet/database hybrid](airtable.md) — Hosted low-code DB / no-code app platform.

### Reference
- [YarentY — public profile & projects summary](yarenty_profile_and_projects_summary.md) — Aggregated bio for cross-linking from other notes.

## See also

- [[../ml/tools/n8n|n8n]] — Workflow-automation engine; pairs with Airtable / Tally. **Lives under `ml/tools/`** because n8n's main use case is now AI agents; the duplicate web-section copy was deleted.
- [[../tools/misc/payments|Hyperswitch]] — Open-source payments switch; **lives under `tools/misc/`**. The duplicate web-section copy was deleted.
- [[../git/github|GitHub profile README]] — Companion to `yarenty_profile_and_projects_summary` here.

## Keywords

`#web` `#http` `#https` `#web-server` `#ssg` `#no-code` `#profile`
