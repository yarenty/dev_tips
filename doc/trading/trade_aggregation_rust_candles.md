---
title: Trade Aggregation
main_link: https://github.com/MathisWellmann/trade_aggregation-rs?tab=readme-ov-file
keywords: [trade-aggregation-rust-candles, candle, aggregation, trade, mathiswellmann]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Trade Aggregation

**Main link:** <https://github.com/MathisWellmann/trade_aggregation-rs?tab=readme-ov-file>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[a_basic_algo_trading_system_in_rust_part_iv_backtesting]] — A Basic Algo Trading System In Rust: Part IV: Backtesting _(score 22.3)_
- [[a_basic_algo_trading_system_in_rust_part_i]] — A Basic Algo Trading System In Rust: Part I _(score 22.3)_
- [[hummingbot_python]] — hummingbot _(score 22.3)_
- [[mql5]] — MQL5 _(score 22.3)_
- [[rust_algo_trading]] — Algo trading _(score 22.3)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#trade-aggregation-rust-candles` `#trading` `#candle` `#aggregation` `#trade` `#mathiswellmann`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Trade Aggregation

https://github.com/MathisWellmann/trade_aggregation-rs?tab=readme-ov-file

A high performance, modular and flexible trade aggregation crate, producing Candle data, suitable for low-latency applications and incremental updates. It allows the user to choose the rule dictating how a new candle is created through the AggregationRule trait, e.g: Time, Volume based or some other information driven rule. It also allows the user to choose which type of candle will be created from the aggregation process through the ModularCandle trait. Combined with the Candle macro, it enables the user to flexibly create any type of Candle as long as each component implements the CandleComponent trait. The aggregation process is also generic over the type of input trade data as long as it implements the TakerTrade trait, allowing for greater flexibility for downstream projects.

See MathisWellmann/go_trade_aggregation for a go implementation with less features and performance.
