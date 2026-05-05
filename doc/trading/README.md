---
title: Trading
keywords: [trading, algo-trading, market-making, backtesting]
status: reviewed
---

# Trading

Notes on algorithmic trading frameworks, market-making bots, and the bridge between brokerages and code. Mostly Rust, with some Python where the broker world demands it (MetaTrader, crypto exchanges).

The articles here split roughly along three axes:

- **Pedagogy vs. production.** [[folbrecht_algo_trading_series]] is a from-`fn main` walkthrough; [[barter]] is what you reach for once you've outgrown your own toy.
- **Sync vs. async.** Folbrecht is sync threads + crossbeam channels; [[barter]] is tokio-async; [[bot_rust]] (tickgrinder) is the historical pre-`async/await` futures-rs world.
- **Rust vs. Python.** [[hummingbot_python]] dominates the crypto market-making niche; [[mql5]] is the bridge into MetaTrader 5 / FX from Python.

## Articles

- [[folbrecht_algo_trading_series]] — A Basic Algo Trading System in Rust (consolidated 4-part Medium series by Paul Folbrecht). Workspace layout, trait-based DI, Tradier broker, Bollinger mean-reversion, Part-IV backtester.
- [[barter]] — Open-source Rust event-driven trading framework. Multi-threaded `Engine` driving N `Trader`s with shared `Portfolio`; trait-pluggable `Data` / `Strategy` / `Execution`.
- [[bot_rust]] — *Building an Algorithmic Trading Platform in Rust* (Casey Primozic / tickgrinder, 2017). Microservice-flavoured Rust platform on `futures-rs` + Redis pub/sub + PostgreSQL. Historical artifact of the pre-`async/await` era.
- [[hummingbot_python]] — Python framework for automated bots on crypto CEX + DEX exchanges. Market-making oriented, Apache 2.0, large connector catalogue.
- [[trade_aggregation_rust_candles]] — `trade_aggregation-rs` crate. Modular trade-tick → candle aggregation with pluggable `AggregationRule` and `ModularCandle` traits. Pairs naturally with [[barter]].
- [[mql5]] — MQL5 + Python bridge: MQL5's TCP socket client connecting to a Python `socket` server for off-EA computation, plus the `MetaTrader5` Python package for direct quote access. Caveat: socket bridge doesn't run in MT5's Strategy Tester.

## Keywords

`#trading` `#algo-trading` `#rust` `#python` `#backtesting` `#market-making` `#metatrader5` `#crypto`
