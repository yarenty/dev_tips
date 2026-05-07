---
title: "Hyperswitch — open-source payments switch (Stripe, but you control the routing)"
main_link: https://github.com/juspay/hyperswitch
keywords: [hyperswitch, payments, stripe-alternative, juspay, open-source, fintech, rust, payment-orchestration]
status: reviewed
---

# Hyperswitch — open-source payments switch

**Main link:** <https://github.com/juspay/hyperswitch>

## Summary

Hyperswitch is an open-source, Rust-built **payment orchestration layer** by Juspay — one API in front of 50+ payment processors (Stripe, Braintree, Adyen, PayPal, Klarna, regional PSPs across 130+ countries). You integrate Hyperswitch once; behind it you can route by cost, success rate, geography, or processor health, and add or swap processors without touching client code. Available as self-hosted OSS and as a managed/hosted product with extra enterprise features.

## Insight

Reach for Hyperswitch when:

- You already use **Stripe** but want a fallback/secondary processor (resilience, cost arbitrage, regional coverage) without rewriting your checkout.
- You operate in **multiple regions** and need different processors per country.
- You need **smart routing / failover** to claw back revenue lost to declined transactions.
- You want a payment integration you can **self-host and audit**, not a vendor lock-in.

Think of it as "Stripe's API, but you choose what's behind it." It also normalises the wildly inconsistent webhook/refund/dispute formats across processors, which alone is worth the integration effort if you've ever shipped multi-PSP code by hand.

Comparisons:

- vs **Stripe directly** — Stripe is simpler if you're happy being mono-processor.
- vs **Adyen / Checkout.com** — multi-processor but commercial; Hyperswitch is open-source and processor-agnostic.
- vs **Spreedly / IXOPAY** — closed-source competitors in the orchestration space.
- vs **rolling your own** — don't.

Caveats: it's still a relatively young open-source project; the hosted version is where the operational polish (compliance, dashboards, 24×7 support) lives. Self-hosting means you own PCI scope considerations.

## Similar / related topics

- [Stripe](https://stripe.com/) — the developer-experience benchmark; closed.
- [Adyen](https://www.adyen.com/) — enterprise multi-processor competitor.
- [Spreedly](https://www.spreedly.com/) — payments orchestration SaaS.
- [Medusa](https://medusajs.com/) — open-source commerce platform; complementary, not competitive.
- [Saleor](https://saleor.io/) — open-source headless commerce.

## Internal links

- [[oracle_free_tier]] — cheap place to host a self-hosted Hyperswitch instance.
- [[k3s]] — lightweight Kubernetes target for the deployment.
- [[prometheus]] — scrape Hyperswitch metrics for routing/success-rate dashboards.
- [[observability/grafana|grafana]] — visualise those metrics.

## Keywords

`#hyperswitch` `#payments` `#stripe-alternative` `#juspay` `#open-source` `#fintech` `#rust` `#payment-orchestration` `#misc` `#tools`

## References / raw notes

> # The open-source payments switch
>
> The single API to access payment ecosystems across 130+ countries.
>
> Hyperswitch is a community-led, open payments switch to enable access to the best payments infrastructure for every digital business.

Using Hyperswitch you can:

- ⬇️ Reduce dependency on a single processor like Stripe or Braintree
- 🧑‍💻 Reduce dev effort by 90% to add & maintain integrations
- 🚀 Improve success rates with seamless failover and auto-retries
- 💸 Reduce processing fees with smart routing
- 🎨 Customise payment flows with full visibility and control
- 🌐 Increase business reach with local / alternate payment methods

### 🌟 Supported payment processors and methods

As of Aug 2024, Hyperswitch supports **50+ payment processors** and multiple global payment methods. New processors are integrated continuously based on reach and community requests; the stated target is **100+ processors by H2 2024**. The latest list of payment processors, supported methods, and features lives in the project docs.

### 🌟 Hosted version (over OSS)

In addition to all features of the open-source product, the hosted version provides:

**System performance & reliability**

- Scalable to support 50 000 tps
- System uptime up to 99.99%
- Low-latency deployment
- Hosting on AWS or GCP
- Value-added services

**Compliance support (PCI, GDPR, card vault, etc.)**

- Customise the integration or payment experience
- Control Center with elaborate analytics and reporting
- Integration with risk-management solutions
- Integration with other platforms (subscription, e-commerce, accounting, etc.)
- Enterprise support

**24 × 7 email / on-call support**

- Dedicated relationship manager
- Custom dashboards with deep analytics, alerts, and reporting
- Expert team to consult and improve business metrics

Repo: <https://github.com/juspay/hyperswitch>
