---
title: "ngrok — instant HTTPS tunnel to localhost"
main_link: https://ngrok.com/
keywords: [ngrok, tunnel, webhook, https, basic-auth, oauth, localhost, reverse-proxy]
status: reviewed
---

# ngrok — instant HTTPS tunnel to localhost

**Main link:** <https://ngrok.com/>

Docs: <https://ngrok.com/docs> · Setup: <https://dashboard.ngrok.com/get-started/setup>

## Summary

ngrok is a hosted reverse-proxy service: you run `ngrok http 8000` and it gives you back a public `https://<random>.ngrok-free.app` URL that tunnels straight into your local port. It handles TLS termination, exposes a `localhost:4040` request inspector, and adds optional auth (Basic, OAuth, OIDC, SAML) without changing your app. Originally a one-binary side project, it's now a freemium SaaS company (random subdomains on the free tier, reserved domains on paid).

## Insight

The killer use case is webhook development — you point Stripe, Twilio, GitHub, or DocuSign at an ngrok URL, see every request hit your laptop, and use **request replay** at `localhost:4040` to re-fire the same payload after each code change. That alone removes the "click button → wait for callback → realise the JSON parser blew up → repeat" loop.

Gotchas and trade-offs:

- **Free tier subdomains are random** and rotate every restart, so anything that needs a stable URL (a webhook registered in a third-party dashboard) effectively requires a paid plan or a workaround.
- **TLS is fine, but you're routing all traffic through ngrok's edge** — that's a vendor and a privacy choice. Don't tunnel anything sensitive through someone else's reverse proxy without thinking about it.
- **For long-running self-hosted exposure**, ngrok is the wrong shape. Use [[rathole]], [frp](https://github.com/fatedier/frp), [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/), or a [[wireguard]] mesh to a cheap VPS instead.
- **Modern competitors** that often beat it for specific cases: [Tailscale Funnel](https://tailscale.com/kb/1223/funnel) (zero-config if you're already on Tailscale), [bore](https://github.com/ekzhang/bore) (tiny self-hostable Rust binary), Cloudflare Tunnel (free, no random URL), [localtunnel](https://localtunnel.github.io/www/), [serveo](https://serveo.net/).

## Similar / related topics

- [[rathole]] — self-hosted Rust reverse-proxy; the "I own a VPS" answer to ngrok.
- [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) — free, persistent, runs on your domain.
- [Tailscale Funnel](https://tailscale.com/kb/1223/funnel) — expose a tailnet node to the public internet.
- [bore](https://github.com/ekzhang/bore) — minimal Rust tunneller; ~500 LOC, self-hosts in a minute.
- [frp](https://github.com/fatedier/frp) — Go reverse-proxy with a huge feature surface; the older incumbent rathole was written to replace.

## Internal links

<!-- reviewed -->

- [[rathole]]
- [[wireguard]]
- [[sniffnet]]
- [[oracle_free_tier]]

## Keywords

`#ngrok` `#tunnel` `#webhook` `#reverse-proxy` `#https` `#oauth` `#localhost` `#tools`

## References / raw notes

### Run

```shell
ngrok http 80
```

### HTTP Basic Auth in one flag

```shell
ngrok http 8000 --basic-auth="katelibby:rooftoppool"
```

Single username/password in front of the tunnel — fine for a one-off demo, never for production.

### OAuth 2.0 (Google) with domain restriction

```shell
ngrok http 3001 --oauth=google
ngrok http 3001 --oauth=google --oauth-allow-domain=company.com
ngrok http 3001 --oauth=google --oauth-allow-domain=company.com,customer.com
ngrok http 3001 --oauth=google --oauth-allow-domain=company.com,customer.com --oauth-allow-email=first.last@contractor.com
```

ngrok injects identity headers into the proxied request:

```text
Ngrok-Auth-User-Email: keith@ngrok.com
Ngrok-Auth-User-Id:    102528612345998048947
Ngrok-Auth-User-Name:  Keith Casey
Referer: https://accounts.google.com/
```

### OIDC (e.g. Okta)

In the Okta admin dashboard, create an OpenID Connect app using the Authorization Code flow and add `https://idp.ngrok.com/oauth2/callback` as the sign-in redirect URI. Then:

```shell
ngrok http 3001 \
  --oidc=https://mycompany.okta.com \
  --oidc-client-id=myclientid \
  --oidc-client-secret=myclientsecret
```

### Request inspector & replay

When ngrok starts it prints two URLs: the public tunnel and `http://localhost:4040`, the **Request Inspector**. Every request through the tunnel is logged with full headers and body. For each one you get:

- **Replay** — re-fires the exact same request into your app (does *not* spoof source IP or webhook signatures).
- **Replay with Modifications** — same request as a template, edit method/headers/body before re-firing.

This is the single feature that makes ngrok worth tolerating for webhook work: the dev loop collapses from "trigger external event → wait → debug" to "click Replay → debug".
