---
title: "patchbay.pub: serverless HTTP pub/sub via curl"
main_link: https://patchbay.pub/
keywords: [patchbay, pubsub, http, serverless, webhooks, no-auth, hobby-tools]
status: reviewed
---

# patchbay.pub: serverless HTTP pub/sub via curl

**Main link:** <https://patchbay.pub/>

## Summary

[patchbay.pub](https://patchbay.pub/) is a tiny free web service that turns any URL into either a one-shot rendezvous channel (one consumer, one producer) or a pub/sub broadcast channel — accessed entirely via plain `curl` requests, with no account, no auth, and no server to run. A blocking `GET https://patchbay.pub/<channel-id>` waits until a `POST` to the same URL hands over a payload, and the payload is delivered to the GET as the response body. Use the `/pubsub/` prefix and the same channel becomes a multi-consumer broadcast where the POST is non-blocking.

## Insight

Reach for patchbay when you need to **glue two scripts together over HTTP without writing a server**:

- **Cross-machine notifications** — `curl https://patchbay.pub/your-secret-id` on your phone (via Termux or Shortcuts) blocks until your laptop POSTs to it. Push notifications without Firebase.
- **One-off file sharing** — `curl --data-binary @file.tar.gz https://patchbay.pub/abc-123` on one box, `curl https://patchbay.pub/abc-123 > file.tar.gz` on the other.
- **Webhook dev** — point a service-under-test at a patchbay URL and `curl` it from your laptop to see what arrives.
- **Cross-platform IPC for hacks** — bots, smart-home glue, IoT widgets where bringing in MQTT or NATS is overkill.

Don't use it for anything you care about:

- **No auth** — anyone who guesses or sees your channel ID can read or write to it. Use long random IDs (the homepage gives you one), but treat them as semi-public.
- **No persistence** — messages are delivered to whoever is currently waiting. If nobody's connected, the message is gone.
- **No SLA** — it's a single hobby service run by [Anders Pitman](https://github.com/anderspitman). If you need reliability, run your own ([source is on GitHub](https://github.com/patchbay-pub/patchbay)).

For anything beyond hobby use, reach for [[messaging/mqtt|MQTT]] (sensor-shaped fan-in), [[programming/rust/net/nats|NATS]] (cluster-shaped pub/sub), or [webhook.site](https://webhook.site) (just for inspecting webhooks).

## Similar / related topics

- [webhook.site](https://webhook.site/) — slicker UI for the "give me a URL to see what gets POSTed" use case; not a pub/sub bus.
- [ngrok](https://ngrok.com/) / [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/) — tunnel a real local server through a public URL; very different shape but similar "I just need a public endpoint" need.
- [[messaging/mqtt|mqtt]] — proper IoT pub/sub bus when you outgrow patchbay.
- [NATS](https://nats.io/) — proper cluster pub/sub bus for the same reason.
- [[oracle_free_tier]] — where to host your own patchbay-style service if you don't want to depend on a third party.

## Internal links

- [[messaging/mqtt|mqtt]] — proper IoT pub/sub when you outgrow patchbay
- [[oracle_free_tier]] — host your own equivalent for free

## Keywords

`#messaging` `#patchbay` `#pubsub` `#http` `#serverless` `#webhooks` `#no-auth` `#hobby-tools`

## References / raw notes

### Pitch (paraphrased from the site)

> patchbay.pub is a free web service you can use to implement things like static site hosting, file sharing, cross-platform notifications, webhooks handling, smart home event routing, IoT reporting, job queues, chat systems, bots, etc, all completely serverless and requiring no account creation or authentication. Most implementations need nothing but `curl` and simple bash snippets.

### Trivial example: rendezvous channel (one consumer, one producer)

In one terminal — this blocks until a producer arrives:

```sh
curl https://patchbay.pub/8ew3-s004
```

In another — POST hands payload to the blocked GET:

```sh
curl https://patchbay.pub/8ew3-s004 -d "Hi there"
```

The blocked GET unblocks and prints `Hi there`.

### `/pubsub/` channel (one publisher, many subscribers, broadcast)

Same channel ID under the `/pubsub/` prefix:

```sh
# In N consumer terminals
curl https://patchbay.pub/pubsub/8ew3-s004

# In a producer terminal
curl https://patchbay.pub/pubsub/8ew3-s004 -d "Hi there"
```

The POST is now non-blocking and broadcasts the payload to every blocked GET.

### Useful entry points

- Site: <https://patchbay.pub/>
- Source: <https://github.com/patchbay-pub/patchbay>
- Author: <https://github.com/anderspitman>
- Discussion / examples (HN): <https://news.ycombinator.com/from?site=patchbay.pub>
