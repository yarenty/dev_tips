---
title: trade_aggregation-rs
main_link: https://github.com/MathisWellmann/trade_aggregation-rs
keywords: [trade-aggregation, rust, candles, ohlc, market-data, low-latency]
status: reviewed
---

# trade_aggregation-rs

**Main link:** <https://github.com/MathisWellmann/trade_aggregation-rs>

Go counterpart: <https://github.com/MathisWellmann/go_trade_aggregation>

## Summary

A small, modular Rust crate by [Mathis Wellmann](https://github.com/MathisWellmann) that aggregates a stream of trade ticks into candles. Three traits do all the work: `TakerTrade` (input — anything that looks like a trade), `AggregationRule` (when to close the current candle — by time, by volume, by ticks, by anything), and `ModularCandle` + `CandleComponent` (what to put *in* the candle — OHLC, VWAP, your own fields). A `Candle` macro stitches arbitrary components into a single struct. Designed for low-latency, incremental updates rather than batch DataFrame work.

## Insight

Trade-to-candle aggregation is one of those things every trading system reimplements badly. This crate is worth using (or at least reading) because it separates the three independent decisions — *what's the input shape*, *when do we cut a candle*, *what does a candle contain* — into three traits, instead of hard-coding "1-minute OHLC from `(price, qty)` ticks" the way 90% of homegrown aggregators do. That means you can swap a time-based rule for a volume-based or imbalance-based rule without touching candle-construction code, which is exactly what you need when you start playing with non-time-uniform bars (volume bars, dollar bars, tick imbalance bars — see López de Prado's *Advances in Financial Machine Learning*, ch. 2).

It pairs naturally with [[barter]]'s `MarketGenerator` (or with the live websocket layer in [[folbrecht_algo_trading_series]]) where it sits one level upstream of strategy code: trades come off the websocket, this crate emits candles, the strategy consumes candles.

## Similar / related topics

- [[barter]] — Rust trading framework; this crate slots in upstream of `MarketGenerator`.
- [[folbrecht_algo_trading_series]] — Folbrecht builds his own minimal aggregator inline; trade_aggregation-rs is the "do it properly" version.
- [[candle]] — separate Hugging Face ML framework that unfortunately shares the name; not related.
- [Polars](https://pola.rs/) — for batch / DataFrame-style aggregation when latency doesn't matter.

## Internal links

- [[barter]] — pairs naturally as upstream of `MarketGenerator`
- [[folbrecht_algo_trading_series]] — has a hand-rolled equivalent inline
- [Polars](https://pola.rs/) — batch-style alternative when latency is not a concern

## Keywords

`#trading` `#rust` `#candles` `#ohlc` `#market-data` `#low-latency` `#aggregation`

## References / raw notes

From the project description:

> A high performance, modular and flexible trade aggregation crate, producing Candle data, suitable for low-latency applications and incremental updates. It allows the user to choose the rule dictating how a new candle is created through the `AggregationRule` trait, e.g.: Time, Volume based or some other information driven rule. It also allows the user to choose which type of candle will be created from the aggregation process through the `ModularCandle` trait. Combined with the `Candle` macro, it enables the user to flexibly create any type of `Candle` as long as each component implements the `CandleComponent` trait. The aggregation process is also generic over the type of input trade data as long as it implements the `TakerTrade` trait, allowing for greater flexibility for downstream projects.

The Go sibling project (`go_trade_aggregation`) exists but per the author has fewer features and lower performance, so prefer this Rust version when you have the choice.
