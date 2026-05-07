---
title: Strawberry browser — AI-driven browser automation product
main_link: https://strawberrybrowser.com/
keywords: [strawberry-browser, browser-agent, computer-use, web-automation, ai-browser]
status: reviewed
---

# Strawberry browser — AI-driven browser automation product

**Main link:** <https://strawberrybrowser.com/>

## Summary

Strawberry is a "self-driving browser" — a consumer/prosumer product that wraps a browser engine with an LLM-driven agent loop so it can run multi-step web workflows (research, form-filling, data scraping, repetitive tasks) on your behalf. It's *not* the OpenAI codename "Strawberry" that became o1 (a frequent confusion since both names landed in 2024); this is a separate, named-after-the-fruit consumer product in the broader **AI browser / browser-agent** category.

## Insight

Strawberry sits in the rapidly-crowding "agentic browser" space alongside **Arc Search / Browser Company's Dia**, **Perplexity Comet**, **The Browser Company's Dia**, **Brave Leo**, plus the more general **computer-use** APIs (Anthropic's *Computer Use*, OpenAI's *Operator*, Google's *Project Mariner*) that automate browsers from outside. The category is moving fast and the product set will look different in 6 months — treat any specific recommendation as time-stamped.

When picking a browser-automation approach, the practical axis is **trust boundary**: a consumer browser like Strawberry runs in your normal session with your cookies and credentials (convenient, but the agent has full access to logged-in services); the API-based computer-use approach runs in an isolated VM (slower, you re-auth, but the blast radius is contained). For developer-side scripted automation rather than consumer use, Playwright + LLM (or the `browser-use` library) usually beats whatever consumer browser is hot this quarter.

If you arrived here looking for **OpenAI's o1 / o3** (codenamed *Strawberry* during development, *Q\** earlier still), see [[deepseek]] and [[../inspection/leaderboards|leaderboards]] for the reasoning-model landscape — there is no dedicated article in this vault since the product is just "ChatGPT with a 'thinking' toggle".

## Similar / related topics

- Anthropic *Computer Use* — API-level browser/desktop automation in an isolated VM.
- OpenAI *Operator* — agent-as-a-service that drives a remote browser.
- Perplexity *Comet* — Perplexity's own browser-agent product.
- The Browser Company *Dia* — Arc successor with built-in AI workflows.
- `browser-use` / Playwright + LLM — developer-facing recipes for building your own.

## Internal links
- [[README|llm/models]]
- [[../README|llm]]
- [[../../agents/README|agents]]
- [[llm_os|llm/llm_os]] — the LLM-OS framing puts browser-as-syscall.

## Keywords

`#strawberry-browser` `#browser-agent` `#computer-use` `#web-automation` `#ai-browser`

## References / raw notes

- Project home: <https://strawberrybrowser.com/>

> The self-driving browser. Strawberry brings intuitive AI automation to your existing workflows.

### Naming-collision note

The codename **"Strawberry"** was widely reported (Reuters, *The Information*, etc., mid-2024) as OpenAI's internal name for what shipped as **o1** in September 2024. That model is unrelated to this product despite sharing the name; this article is about the browser only.
