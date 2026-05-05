---
title: HTTPS for websites — Certbot + Apache
main_link: https://certbot.eff.org/
keywords: [https, ssl, tls, certbot, lets-encrypt, apache, certificates]
status: reviewed
---

# HTTPS for websites — Certbot + Apache

**Main link:** <https://certbot.eff.org/>

## Summary

Cheat-sheet for getting **HTTPS** on a website hosted by Apache, using **Certbot** + **Let's Encrypt**. Two commands: install `certbot` with the Apache plugin, then run `sudo certbot --apache` — it issues a free 90-day cert via Let's Encrypt, edits the Apache vhost config to add HTTPS, and registers a systemd timer for auto-renewal. Works for nginx, Caddy, and standalone too with the appropriate plugin.

## Insight

Use this whenever you need HTTPS on a server you control. **Free, automated, ubiquitous** — Let's Encrypt issues over a million certs a day; the auto-renewal cron means you set it up once and forget. The whole flow takes ~2 minutes.

When *not* to reach for Certbot:
- **Behind Cloudflare / managed CDN** — they already terminate TLS for you with their own certs; you only need an "origin cert" (theirs is also free).
- **Local development** — use `mkcert` to generate locally-trusted certs without going to Let's Encrypt at all.
- **Caddy** — it does ACME / Let's Encrypt natively without Certbot. If you're starting fresh, Caddy is the easier path.

Gotcha: certs are valid 90 days; if your renewal cron breaks, sites go down. `certbot renew --dry-run` regularly to verify.

## Similar / related topics

- [[apache]] — Apache HTTPD cheat-sheet (you'll need Apache running before HTTPS).
- **Caddy** — alternative web server with ACME built-in; no Certbot needed.
- **`mkcert`** — locally-trusted certs for dev/test without Let's Encrypt.
- **Cloudflare TLS** — managed alternative; free origin certs.
- **`acme.sh`** — pure-shell ACME client; tiny dependency footprint.

## Internal links

<!-- reviewed -->

- [[apache]] — Apache HTTPD cheat-sheet.
- [[signing_mac_libs]] — Different "certificate" topic but adjacent (code signing vs TLS).

## Keywords

`#https` `#ssl` `#tls` `#certbot` `#lets-encrypt` `#apache` `#certificates`

## References / raw notes

- Certbot home (interactive instructions for any web server / OS): <https://certbot.eff.org/>
- Let's Encrypt: <https://letsencrypt.org/>
- ACME protocol (RFC 8555): <https://datatracker.ietf.org/doc/html/rfc8555>
- `mkcert` (local dev): <https://github.com/FiloSottile/mkcert>

### Recipe (Debian/Ubuntu + Apache)

```shell
sudo apt install certbot python3-certbot-apache
sudo certbot --apache              # interactive: pick domains, agree TOS, get cert
sudo certbot renew --dry-run       # verify auto-renewal works
```

That's it. The Apache plugin edits your vhost config to add `Listen 443`, `<VirtualHost *:443>`, `SSLEngine on`, and the cert paths under `/etc/letsencrypt/live/<domain>/`.

### Other web servers

```shell
sudo certbot --nginx               # nginx plugin
sudo certbot --standalone          # spin up a temporary built-in server (port 80 must be free)
sudo certbot certonly              # cert-only, manual config
```

### Renewal

Certbot installs a `systemd` timer (or cron job on older systems) that runs `certbot renew` twice daily. Renewals only proceed if the cert is within 30 days of expiry, so it's safe to run often. Verify with:

```shell
systemctl list-timers | grep certbot
sudo certbot renew --dry-run
```

### Wildcard certs

Need `*.example.com`? Use the DNS-01 challenge with a DNS-API plugin (Cloudflare, Route 53, DigitalOcean, etc.):

```shell
sudo apt install python3-certbot-dns-cloudflare
sudo certbot certonly \
  --dns-cloudflare \
  --dns-cloudflare-credentials ~/.secrets/cloudflare.ini \
  -d 'example.com,*.example.com'
```
