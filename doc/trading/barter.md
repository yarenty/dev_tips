---
title: Barter
main_link: https://github.com/barter-rs/barter-rs
keywords: [barter, rust, trading, event-driven, backtesting, engine, portfolio]
status: reviewed
review_date: 2026/05/03
---

# Barter

**Main link:** <https://github.com/barter-rs/barter-rs>

Crate: <https://crates.io/crates/barter> · Live-trades example: <https://github.com/barter-rs/barter-rs/blob/main/barter/examples/engine_with_live_trades.rs>

## Summary

Barter is an open-source Rust framework for event-driven live trading and backtesting. It ships a multi-threaded `Engine` that drives one or more `Trader`s (one per market pair), each composed of pluggable `Data`, `Strategy`, and `Execution` components plus a shared `Portfolio`. The whole system is wired by traits (`MarketGenerator`, `SignalGenerator`, `OrderGenerator`, `FillUpdater`, `ExecutionClient`, ...) so the *same* trader code runs against historical candles for backtesting and against the [barter-data](https://github.com/barter-rs/barter-data-rs) websocket feed for live trading. Engine control happens via an mpsc `command_tx` (e.g. `Command::Terminate`, `Command::ExitPosition`) and observability via an unbounded `event_rx`.

## Insight

Reach for Barter when you want a Rust framework that already made the unfun decisions for you: per-trader threads, async tokio runtime, an event loop you can event-source, and a statistic module that gives you Sharpe, Calmar, max drawdown, and one-pass dispersion stats out of the box. It maps cleanly onto a "one trader per market pair, shared portfolio across traders" mental model, which matches how most discretionary-style algos actually want to be deployed.

It is *not* the right tool if you want to learn how a trading system is put together — for that, [[folbrecht_algo_trading_series]] is much more readable, because Folbrecht writes everything from `fn main` down. Barter is what you graduate to once you've outgrown your own toy. Compared to [[hummingbot_python]] it's leaner, lower-level, async-native, and lacks the connector zoo (Binance + a handful of others via barter-data, vs. Hummingbot's CEX+DEX firehose).

## Similar / related topics

- [[folbrecht_algo_trading_series]] — pedagogical from-scratch Rust trading system; nice contrast in scope.
- [[bot_rust]] — tickgrinder, an older futures-rs platform; predecessor-style design.
- [[trade_aggregation_rust_candles]] — modular candle aggregation that complements Barter's `MarketGenerator`.
- [[hummingbot_python]] — Python equivalent with broader exchange coverage.
- [[tokio]] — async runtime Barter is built on.

## Internal links

- [[folbrecht_algo_trading_series]] — sync, hand-rolled Rust counterpart
- [[bot_rust]] — futures-rs based predecessor-style platform
- [[trade_aggregation_rust_candles]] — candle aggregation primitives
- [[tokio]] — async runtime
- [[hummingbot_python]] — Python market-making framework

## Keywords

`#trading` `#rust` `#barter` `#event-driven` `#backtesting` `#engine` `#portfolio` `#tokio`

## References / raw notes

Per the project's overview, Barter is decomposed into the following trait-driven components:

- **Data** — `MarketGenerator` produces `MarketEvent`s (the system heartbeat). `live::MarketFeed` uses barter-data websockets; `historical::MarketFeed` replays a candle iterator for backtests.
- **Strategy** — `SignalGenerator` consumes `MarketEvent`s and emits advisory `Signal`s.
- **Portfolio** — `MarketUpdater` + `OrderGenerator` + `FillUpdater` traits govern portfolio state. A `MetaPortfolio` turns `Signal`s into `OrderEvent`s and updates state on `MarketEvent` / `FillEvent`.
- **Execution** — `ExecutionClient` turns `OrderEvent`s into `FillEvent`s. `SimulatedExecution` is provided for dry-runs and backtests.
- **Statistic** — Sharpe, Calmar, max drawdown, one-pass dispersion over closed positions.
- **Trader** — one market pair, owns its own `Data` / `Strategy` / `Execution`, shares the `Portfolio`.
- **Engine** — multi-threaded driver of N `Trader`s, controllable via `command_tx`, observable via `event_rx`.

### Sketch of `main` from the live-trades example

```rust
#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let (command_tx, command_rx) = mpsc::channel(20);
    let (event_tx, event_rx) = mpsc::unbounded_channel();
    let event_tx = EventTx::new(event_tx);
    let engine_id = Uuid::new_v4();

    let market = Market::new("binance", ("btc", "usdt", InstrumentKind::Spot));

    let portfolio = Arc::new(Mutex::new(
        MetaPortfolio::builder()
            .engine_id(engine_id)
            .markets(vec![market.clone()])
            .starting_cash(10_000.0)
            .repository(InMemoryRepository::new())
            .allocation_manager(DefaultAllocator { default_order_value: 100.0 })
            .risk_manager(DefaultRisk {})
            .statistic_config(StatisticConfig {
                starting_equity: 10_000.0,
                trading_days_per_year: 365,
                risk_free_return: 0.0,
            })
            .build_and_init()
            .expect("failed to build & initialise MetaPortfolio"),
    ));

    let (trader_command_tx, trader_command_rx) = mpsc::channel(10);
    let mut traders = vec![
        Trader::builder()
            .engine_id(engine_id)
            .market(market.clone())
            .command_rx(trader_command_rx)
            .event_tx(event_tx.clone())
            .portfolio(Arc::clone(&portfolio))
            .data(historical::MarketFeed::new([test_util::market_candle].into_iter()))
            .strategy(RSIStrategy::new(StrategyConfig { rsi_period: 14 }))
            .execution(SimulatedExecution::new(ExecutionConfig {
                simulated_fees_pct: Fees { exchange: 0.1, slippage: 0.05, network: 0.0 },
            }))
            .build()
            .expect("failed to build trader"),
    ];

    let trader_command_txs = HashMap::from_iter([(market, trader_command_tx)]);

    let engine = Engine::builder()
        .engine_id(engine_id)
        .command_rx(command_rx)
        .portfolio(portfolio)
        .traders(traders)
        .trader_command_txs(trader_command_txs)
        .statistics_summary(TradingSummary::init(StatisticConfig {
            starting_equity: 1000.0,
            trading_days_per_year: 365,
            risk_free_return: 0.0,
        }))
        .build()
        .expect("failed to build engine");

    tokio::spawn(listen_to_events(event_rx));
    engine.run().await;
    Ok(())
}
```

The same `Trader` definition runs against `live::MarketFeed` for live trading — only the `data(...)` builder argument changes.
