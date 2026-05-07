---
title: "Folbrecht: A Basic Algo Trading System in Rust (series)"
main_link: https://github.com/Paul-Folbrecht/algo-trading
keywords: [folbrecht-algo-trading, rust, trading, onion-architecture, backtesting, dependency-injection, mean-reversion]
status: reviewed
review_date: 2026/05/03
---

# Folbrecht: A Basic Algo Trading System in Rust (series)

**Main link:** <https://github.com/Paul-Folbrecht/algo-trading>

**Series on Medium / Rustaceans:**

- Part I — Architecture & market data: <https://medium.com/rustaceans/a-basic-algo-trading-system-in-rust-part-i-c47d10d3bf9b>
- Part II — Strategy & order flow: <https://medium.com/rustaceans/a-basic-algo-trading-system-in-rust-part-ii-c47d10d3bf9c>
- Part III — Config, tests, serialization, MongoDB, DI revisited
- Part IV — Backtesting (and "it actually made money")

## Summary

Paul Folbrecht's four-part Medium series walks through building a small but complete algorithmic trading system in Rust against the [Tradier](https://tradier.com/) brokerage API. The system is deliberately sync (no `async`), uses [crossbeam](https://crates.io/crates/crossbeam) channels and `tungstenite` websockets, and is organized as a Cargo workspace with onion-architecture style layering (`services` → `domain` → `core`). Strategies are pluggable via traits; the included Bollinger-band mean-reversion strategy is shown to backtest at ~63% over two years on AAPL+AMZN vs ~53% for NASDAQ in the same window.

## Insight

This is one of the cleanest "real, non-trivial Rust app" tutorials I've found — far more useful than yet another `tokio` echo server, because it commits to opinionated answers on the questions every Rust app eventually faces:

- **Async or sync?** Sync, because the workload is a handful of channels, not 10k concurrent sockets. Async Rust is a "beast unto itself, best avoided unless you need what it buys you."
- **DI without an OO container?** A `pub trait` + `pub fn new() -> Arc<impl Trait>` constructor + a private `mod implementation` struct. Static dispatch (`impl Trait`) over `dyn Trait` so you keep generics in the interface.
- **Inter-component messaging?** `crossbeam::channel` over native `mpsc` (richer API, same cost), no ZeroMQ — there's no point reaching for an external bus inside a single process.
- **Workspace layout?** Multiple library crates with dependencies pointing strictly inward; `domain` (business logic) has zero I/O deps. The backtester in Part IV is a *separate binary crate* that swaps in alternative `MarketDataService` / `HistoricalDataService` / `OrderService` impls — proving the DI scheme actually buys you something.

The Part IV backtester result (alpha vs. NASDAQ on a stupid-simple Bollinger strategy over a friendly two-year window) should be read as "the framework works", not "ship this to prod" — single strategy, two symbols, one regime.

## Similar / related topics

- [[barter]] — production-grade Rust event-driven trading framework; what you'd reach for instead of rolling your own.
- [[bot_rust]] — older, futures-based Rust trading platform (tickgrinder); contrasts the sync vs. async design choice.
- [[trade_aggregation_rust_candles]] — modular candle aggregation crate, useful upstream of any backtester.
- [[hummingbot_python]] — the Python-world equivalent, market-making oriented, CEX + DEX.
- [[mql5]] — MetaTrader 5 / MQL5 if you'd rather attach to an existing broker terminal.

## Internal links

- [[barter]] — Rust trading engine framework
- [[bot_rust]] — tickgrinder, an earlier futures-based Rust trading platform
- [[trade_aggregation_rust_candles]] — candle aggregation primitives
- [[crossbeam]] — channels used throughout the series
- [[tokio]] — explicitly *not* used; useful as a contrast

## Keywords

`#trading` `#rust` `#algo-trading` `#onion-architecture` `#backtesting` `#dependency-injection` `#mean-reversion` `#tradier`

## References / raw notes

Repo: <https://github.com/Paul-Folbrecht/algo-trading>
Author gists: <https://gist.github.com/Paul-Folbrecht/>

### Architectural decisions, in one place

- **Workspace** with crates: `services`, `domain`, `core`. Dependencies point inward only.
- **Service pattern**: `pub trait FooService` + `pub fn new(...) -> Arc<impl FooService>` + `mod implementation { pub struct Foo { ... } }`. The `Arc` exists so the service can be shared across threads.
- **Static dispatch** (`impl Trait`) over `dyn Trait` — keeps generics in interfaces, no vtable cost. Either is viable; perf delta is negligible at service granularity.
- **No async**. Plain OS threads + channels.
- **Channels**: `crossbeam` unbounded for in-process messaging. The `MarketDataService` owns one websocket and fans out `Quote` events to N subscribers via cloned `Sender`s.
- **HTTP**: `reqwest` blocking + `serde`. **WebSocket**: `tungstenite`.

### Part I: market data + main wiring (excerpt)

```rust
fn main() {
    let config = AppConfig::new().expect("Could not load config");
    let access_token = std::env::args().nth(1).unwrap();
    let market_data_service = market_data::new(access_token.clone());
    let historical_data_service = historical_data::new(access_token);
    let shutdown = Arc::new(AtomicBool::new(false));
    let mut symbols: HashSet<String> = HashSet::new();

    config.strategies.iter().for_each(|strategy| {
        symbols.extend(strategy.symbols.clone());
        let mut trading_service = trading::trading::new(
            strategy.name.clone(),
            strategy.symbols.as_ref(),
            market_data_service.clone(),
            historical_data_service.clone(),
        );
        trading_service.run().unwrap();
    });

    let handle = market_data_service.init(shutdown, symbols.into_iter().collect());
    handle.unwrap().join().unwrap();
}
```

The `MarketDataService` trait + `Arc<impl MarketDataService>` constructor pattern:

```rust
pub trait MarketDataService {
    fn init(&self, shutdown: Arc<AtomicBool>, symbols: Vec<String>) -> Result<JoinHandle<()>, String>;
    fn subscribe(&self) -> Result<Receiver<Quote>, String>;
    fn unsubscribe(&self, subscriber: Receiver<Quote>) -> Result<(), String>;
}

pub fn new(access_token: String) -> Arc<impl MarketDataService> {
    Arc::new(implementation::MarketData {
        access_token,
        socket: None,
        symbols: HashSet::new(),
        subscribers: Arc::new(Mutex::new(Vec::new())),
    })
}
```

### Part III: config, tests, Mongo, DI revisited

Config uses the [`config`](https://crates.io/crates/config) crate, which gives free layered overrides (base + environment-specific) for almost no code:

```toml
sandbox = true
access_token = ""
mongo_url = "mongodb://localhost:27017/"

[[strategies]]
name = "mean-reversion"
symbols = ["AAPL", "AMZN"]
capital = [100000, 10000]
```

Things missing from "real production" but called out honestly:

- A proper ADT error type (uses `String` for errors).
- Clean shutdown (added in a later iteration).

### Part IV: backtesting

The backtester is a **separate binary crate** that reuses the same `domain` and `core`, but swaps in:

- `BacktestMarketDataService` — replays canned historical bars instead of consuming a websocket.
- `BacktestHistoricalDataService` — serves from an in-memory cache pre-loaded once at init.
- `BacktestOrderService` — simulated fills, no broker call.

`TradingService` is parameterized by "current date" so the same strategy code runs in both live and backtest mode. This is the payoff for the DI discipline in Parts I–III: the backtester is a few hundred lines, not a fork.

Backtest result reported in the article: ~62.9% return vs. ~53.5% NASDAQ over the same two-year window, trading AAPL + AMZN with a Bollinger-band mean-reversion strategy on $20k capital. Caveat reader: one strategy, two symbols, one regime, one historical run. The interesting fact is the *framework* worked end to end.
