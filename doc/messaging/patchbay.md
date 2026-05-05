---
title: patchbay.pub
main_link: 
keywords: [patchbay, messaging, pubsub, pub, iot]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# patchbay.pub

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#patchbay` `#messaging` `#pubsub` `#pub` `#iot` `#create`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# patchbay.pub


patchbay.pub is a free web service you can use to implement things like static site hosting, file sharing, cross-platform notifications, webhooks handling, smart home event routing, IoT reporting, job queues, chat systems, bots, etc, all completely serverless and requiring no account creation or authentication. Most implementations need nothing but curl and simple bash snippets.




patchbay.pub


trivial example. If you run this GET to create a consumer, it will block:

```shell

curl https://patchbay.pub/8ew3-s004
```
Until you also run this POST in another terminal to create a producer:

```shell

curl https://patchbay.pub/8ew3-s004 -d "Hi there"
```





If you use the /pubsub/ protocol, like this:

```shell

curl https://patchbay.pub/pubsub/8ew3-s004


curl https://patchbay.pub/pubsub/8ew3-s004 -d "Hi there"
```


the request uses the channel in a different mode. GETs act similarly to before, but POSTs become non-blocking, and will broadcast messages to all blocked pubsub consumers, not just the first one. As the name suggest, this models the PubSub pattern.
