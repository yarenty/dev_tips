---
title: Hummingbot
main_link: https://hummingbot.org/
keywords: [hummingbot, python, trading, market-making, cex, dex, crypto, connectors]
status: reviewed
review_date: 2026/05/03
---

# Hummingbot

**Main link:** <https://hummingbot.org/>

Repo: <https://github.com/hummingbot/hummingbot> · Docs: <https://hummingbot.org/docs/>

## Summary

Hummingbot is an Apache-2.0 licensed Python framework for building automated trading bots — primarily market-making and arb — across crypto exchanges. It maintains a large catalogue of CEX connectors (Binance, KuCoin, ...) and DEX connectors (Uniswap, PancakeSwap, ... on Ethereum, BNB Chain, etc.) and a "V2 Strategies" framework for composing backtestable, multi-venue, multi-timeframe strategies. It runs as a local client on your own machine or VM, with API keys encrypted at rest and never sent to third parties.

## Insight

Hummingbot is the obvious starting point if your problem is "I want to run a market-making or arbitrage bot on a crypto exchange this weekend, not write a trading framework". It hides the websocket-and-REST connector grunt work for the long tail of exchanges, which is *most* of what writing a crypto bot from scratch actually is. The catch is that it's opinionated toward market-making rather than directional trading, and the V2 framework — while more flexible than V1 — still asks you to think in Hummingbot's vocabulary (`Controllers`, `Executors`, `MarketMakingControllerBase`, ...) rather than vanilla async Python.

If you want a Rust counterpart, [[barter]] is the closest analogue but with a much smaller exchange surface (you'd lean on [`barter-data`](https://github.com/barter-rs/barter-data-rs) for connectors). If you want to learn how a trading bot is actually wired together, write a small one yourself with [[folbrecht_algo_trading_series]] as a reference and only adopt Hummingbot once your own toy collapses under the weight of having to maintain N exchange connectors.

## Similar / related topics

- [[barter]] — Rust event-driven trading framework; smaller scope, leaner.
- [[folbrecht_algo_trading_series]] — pedagogical from-scratch Rust trading system.
- [[bot_rust]] — older Rust platform with a similar microservice-flavoured shape.
- [[trade_aggregation_rust_candles]] — candle aggregation primitives in Rust.
- [[mql5]] — MetaTrader 5 Python bridge if you'd rather plug into MT5 instead of crypto exchanges.

## Internal links

- [[barter]] — Rust counterpart, smaller connector zoo
- [[folbrecht_algo_trading_series]] — DIY Rust trading framework
- [[bot_rust]] — older Rust platform
- [[mql5]] — MT5 + Python alternative for FX

## Keywords

`#trading` `#hummingbot` `#python` `#market-making` `#cex` `#dex` `#crypto` `#connectors`

## References / raw notes

Author's pitch (paraphrased from the project README):

- **Both CEX and DEX connectors** — Binance, KuCoin, ... and Uniswap, PancakeSwap, ... across multiple chains.
- **V2 Strategies framework** — composable, backtestable, multi-venue, multi-timeframe strategies.
- **Local client** — runs on your hardware; API keys encrypted locally, never exposed to third parties.
- **Community-maintained connectors** — a global community of quant traders contributes connectors and strategies.

Tangentially related Medium write-up (already had a copy in the raw notes): *I built an automated trading system in Rust for a hedge fund in 6 months* — <https://medium.com/@fintechdevlondon/i-built-an-automated-trading-system-in-rust-for-a-hedge-fund-in-6-months-dfe027fc14ec>.
