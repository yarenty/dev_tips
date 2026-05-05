---
title: A Basic Algo Trading System In Rust: Part III
main_link: 
keywords: [a-basic-algo-trading-system-in-rust-part-iii, trading, rust, config, test]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/trading/rust_algo_trading.md` by `article_split.py`. Heading: **A Basic Algo Trading System In Rust: Part III**.

# A Basic Algo Trading System In Rust: Part III

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#a-basic-algo-trading-system-in-rust-part-iii` `#trading` `#rust` `#config` `#part` `#tests`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# A Basic Algo Trading System In Rust: Part III
Paul Folbrecht
Rustaceans

Jun 30, 2024




This part is dedicated to certain Rust-specific ancillaries I’ve ignored thus far in the interests of space.

We’ll cover the following:

Config
Tests
Serialization
MongoDB
Dependency Injection (Revisited)

## Config
The config format is still pretty simple; here is an example:

```yaml
sandbox = true
access_token = ""
sandbox_token = ""
account_id = ""
mongo_url = "mongodb://localhost:27017/"

[[strategies]]
name = "mean-reversion"
symbols = ["AAPL", "AMZN"]
capital = [100000, 10000]
```

What impressed me out of the gate about Rust’s config crate was the for-free overriding ability. With no effort, one can define a base config that will be overridden — in whole or in part — with another (specific to environment, etc.)

The code:

```rust
use config::{Config, ConfigError, File};
use serde::Deserialize;
use std::{collections::HashMap, env};

#[derive(Debug)]
pub struct AppConfig {
    pub access_token: String,
    pub sandbox_token: String,
    pub account_id: String,
    pub sandbox: bool,
    pub mongo_url: String,
    pub strategies: Vec<Strategy>,
}

impl From<ConfigHolder> for AppConfig {
    fn from(holder: ConfigHolder) -> Self {
        AppConfig {
            access_token: holder.access_token,
            sandbox_token: holder.sandbox_token,
            account_id: holder.account_id,
            sandbox: holder.sandbox,
            mongo_url: holder.mongo_url,
            strategies: holder.strategies.into_iter().map(|s| s.into()).collect(),
        }
    }
}

#[derive(Debug)]
pub struct Strategy {
    pub name: String,
    pub symbols: Vec<String>,
    pub capital: HashMap<String, i64>,
}

impl From<StrategyHolder> for Strategy {
    fn from(holder: StrategyHolder) -> Self {
        let capital = holder
            .symbols
            .iter()
            .zip(holder.capital.iter())
            .map(|(s, c)| (s.clone(), *c))
            .collect();

        Strategy {
            name: holder.name,
            symbols: holder.symbols,
            capital,
        }
    }
}

#[derive(Deserialize)]
struct ConfigHolder {
    pub access_token: String,
    pub sandbox_token: String,
    pub account_id: String,
    pub sandbox: bool,
    pub mongo_url: String,
    pub strategies: Vec<StrategyHolder>,
}

#[derive(Deserialize)]
struct StrategyHolder {
    pub name: String,
    pub symbols: Vec<String>,
    pub capital: Vec<i64>,
}

impl AppConfig {
    pub fn new() -> Result<Self, ConfigError> {
        let run_mode = env::var("RUN_MODE").unwrap_or_else(|_| "development".into());

        let holder: ConfigHolder = Config::builder()
            .add_source(File::with_name("config/default"))
            .add_source(File::with_name(&format!("/config/{}", run_mode)).required(false))
            .add_source(File::with_name("config/local").required(false))
            .build()?
            .try_deserialize::<ConfigHolder>()?;
        Ok(holder.into())
    }
}

```

You can see that I’ve specified “config/default” as the base config, with “config/local” as the optional override (which is where my Tradier tokens live).

The only other interesting aspect of this code is that I chose to deserialize into a temporary set of structs to do the work of translating the raw config artifacts — a pair of symbols and capital arrays per strategy — into the nicer format of a map.

For this purpose I again made use of From implementations to turn the transient forms into the exposed interface.

(Is it possible to do such a translation directly in the deserialization with the library? Perhaps; I didn’t explore that, as this method is simple and straightforward.)

## Tests
I’ve found unit testing in Rust to be a joy thus far.

Perhaps you are familiar with one of Erlang creator Joe Armstrong’s comments regarding object-oriented programming:

Because the problem with object-oriented languages is they’ve got all this implicit environment that they carry around with them. You wanted a banana but what you got was a gorilla holding the banana and the entire jungle.

I have seen many an OO system’s test harnesses fall into this trap, also known as the Uninvited Guest anti-pattern. It arises from the way OO programs tend to be constructed, which is a direct consequence of the fact that OO purposefully entangles the orthogonal concerns of data and functionality. Before you know it, you are feeling this:


Refactorings become painful: A change to a harness for the benefit of one test breaks others, etc.

It’s worth pointing out that this generally just isn’t a concern with the way you’ll construct tests for idiomatic Rust code.

In addition to that, I love how everything regarding unit (& integration) testing is so simple and straightforward, how everything just works with no wasted effort, and how fast it is — pretty much like everything Rust.

Here are a couple examples:

```rust

use super::*;

#[test]
fn test_mean_reversion_strategy() {
    let strategy = Strategy::new("mean-reversion", vec!["SPY".to_string()]);
    let symbol_data = SymbolData {
        mean: 100.0,
        std_dev: 4.714045207910316,
        symbol: "SPY".to_string(),
        history: Vec::new(),
    };

    // Ask is < mean - 2.0 * std_dev so should generate a buy signal
    let buy_quote = Quote {
        symbol: "SPY".to_string(),
        bid: 90.0,
        ask: 90.0,
        biddate: Local::now(),
        askdate: Local::now(),
    };
    match strategy.handle(&buy_quote, &symbol_data) {
        Ok(signal) => {
            assert_eq!(signal, Signal::Buy);
        }
        Err(e) => {
            eprintln!("Error: {}", e);
        }
    }

    // Ask is > mean - 2.0 * std_dev so should generate a sell signal
    let buy_quote = Quote {
        symbol: "SPY".to_string(),
        bid: 150.0,
        ask: 150.0,
        biddate: Local::now(),
        askdate: Local::now(),
    };
    match strategy.handle(&buy_quote, &symbol_data) {
        Ok(signal) => {
            assert_eq!(signal, Signal::Sell);
        }
        Err(e) => {
            eprintln!("Error: {}", e);
        }
    }

    // Ask is within mean - 2.0 * std_dev so should generate no signal
    let buy_quote = Quote {
        symbol: "SPY".to_string(),
        bid: 95.0,
        ask: 95.0,
        biddate: Local::now(),
        askdate: Local::now(),
    };
    match strategy.handle(&buy_quote, &symbol_data) {
        Ok(signal) => {
            assert_eq!(signal, Signal::None);
        }
        Err(e) => {
            eprintln!("Error: {}", e);
        }
    }
}
```

We might expect domain-level unit tests to be simple (no/few dependencies); here’s a service test suite which includes mocking service dependencies:

```rust
use super::*;
use chrono::{Local, NaiveDate};
use domain::domain::{Day, History};
use implementation::*;

struct MockHistoricalDataService {}
impl HistoricalDataService for MockHistoricalDataService {
    fn fetch(&self, _: &str, _: NaiveDate, _: NaiveDate) -> Result<History, reqwest::Error> {
        Ok(History {
            day: vec![
                Day {
                    date: NaiveDate::from_ymd_opt(2024, 4, 1).unwrap(),
                    open: 1.0,
                    high: 1.0,
                    low: 1.0,
                    close: 10.0,
                    volume: 1,
                },
                Day {
                    date: NaiveDate::from_ymd_opt(2024, 4, 2).unwrap(),
                    open: 2.0,
                    high: 2.0,
                    low: 2.0,
                    close: 10.0,
                    volume: 2,
                },
                Day {
                    date: NaiveDate::from_ymd_opt(2024, 4, 3).unwrap(),
                    open: 3.0,
                    high: 3.0,
                    low: 3.0,
                    close: 20.0,
                    volume: 3,
                },
            ],
        })
    }
}

#[test]
fn test_load_history() {
    let symbols = vec!["SPY".to_string()];
    let historical_data_service = Arc::new(MockHistoricalDataService {});
    let data = load_history(&symbols, historical_data_service);
    let spy = data.get(&"SPY".to_string()).unwrap();
    assert_eq!(spy.mean, 13.333333333333334);
    assert_eq!(spy.std_dev, 4.714045207910316);
}

struct MockOrderService {}
impl OrderService for MockOrderService {
    fn create_order(&self, order: Order) -> Result<Order, String> {
        Ok(order.with_id(1000))
    }

    fn get_position(&self, symbol: &str) -> Option<Position> {
        match symbol {
            "SPY" => Some(Position {
                symbol: symbol.to_string(),
                quantity: 100,
                broker_id: None,
                cost_basis: 10000.0,
                date: Local::now(),
            }),
            "AMZN" => None,
            _ => None,
        }
    }

    fn update_position(&self, position: &Position) {
        unimplemented!()
    }
}

#[test]
fn test_handle_quote() {
    let orders = Arc::new(MockOrderService {});

    let quote = Quote {
        symbol: "SPY".to_string(),
        bid: 80.0,
        ask: 80.0,
        biddate: Local::now(),
        askdate: Local::now(),
    };

    match maybe_create_order(Signal::Buy, orders.get_position("SPY"), &quote, 10000) {
        Some(order) => {
            assert_eq!(order.symbol, "SPY");
            // Capital of $10K - 100 shares * 80 = $2000 remaining capital = 25 shares at $80
            assert_eq!(order.quantity, 25);
            assert_eq!(order.side, Side::Buy);
        }
        None => panic!("Expected an order"),
    }

    match maybe_create_order(Signal::Sell, orders.get_position("SPY"), &quote, 10000) {
        Some(order) => {
            assert_eq!(order.symbol, "SPY");
            // We always unwind completely and have 100 shares, so any Sell signal should sell all
            assert_eq!(order.quantity, 100);
            assert_eq!(order.side, Side::Sell);
        }
        None => panic!("Expected an order"),
    }

    match maybe_create_order(Signal::None, orders.get_position("SPY"), &quote, 10000) {
        Some(_) => panic!("Expected no order"),
        None => {}
    }
}

```

## Serialization
I needed to write several custom serde codecs, for either or both HTTP/BSON serialization. Here is one of them:

```rust
use chrono::{DateTime, FixedOffset, Local};
use serde::{self, Deserialize, Deserializer};

pub fn serialize<S>(date: &DateTime<Local>, serializer: S) -> Result<S::Ok, S::Error>
where
    S: serde::Serializer,
{
    serializer.serialize_i64(date.timestamp_millis())
}

pub fn deserialize<'de, D>(deserializer: D) -> Result<DateTime<Local>, D::Error>
where
    D: Deserializer<'de>,
{
    // RFC3339 format: 2024-06-17T13:45:27.304Z
    String::deserialize(deserializer)
        .and_then(|s| {
            DateTime::<FixedOffset>::parse_from_rfc3339(&s).map_err(serde::de::Error::custom)
        })
        .map(|utc| utc.with_timezone(&Local))
}
```

Note that serde doesn’t use reflection or anything with a runtime penalty: It’s done using traits and macros, entirely at compile-time, and is fast.

Here is an example of usage:

```rust

use chrono::{DateTime, Local, NaiveDate};
use core::serde::{millis_date_time_format, rfc_3339_date_time_format, string_date_format};
use serde::{Deserialize, Serialize};

...

#[derive(Deserialize)]
pub struct TradierPosition {
    pub id: i64,
    pub symbol: String,
    pub quantity: f64,
    pub cost_basis: f64,
    #[serde(with = "rfc_3339_date_time_format")]
    pub date_acquired: DateTime<Local>,
}
```

Note that the parameter to the serde macro is actually the module name.

It’s also worth mentioning that core, where these codecs are housed, is a library crate, and can be published and used standalone. This is a consequence of the project’s use of the workspace pattern.
```yaml
[workspace]
members = ["core", "domain", "services", "server"]

[workspace.package]
version = "0.1.0"
authors = ["Paul Folbrecht <paul.folbrecht@gmail.com>"]
edition = "2021"
documentation = ""

[workspace.dependencies]
```

## Mongo
The persistence layer’s code has been covered.

To use the project locally, one must also have a MongoDB instance running at the URL pointed to by the mongo_url config property.

Mongo is very simple to install and run locally. After doing so, the shell can be used to easily manage and query the database:

![img_4.png](../assets/img/trading/img_4.png/img/trading/img_4.png)

(Eventually, the project will be containerized and runnable on AWS, with the mongo instance running in its own container.)

## Dependency Injection (Revisited)
I covered my reasoning about and path for DI in Part I. Now that the system has achieved a non-trivial level of functionality, let’s take a look at what constructing and wiring services & their dependencies looks like in practice:

```rust

fn main() {
    let config = AppConfig::new().expect("Could not load config");
    println!("Config:\n{:?}", config);

    let access_token = config.access_token;
    let sandbox_token = config.sandbox_token;
    let market_data = market_data::new(access_token.clone());
    let historical_data = historical_data::new(access_token.clone());
    let persistence = persistence::new(config.mongo_url.clone());
    let orders = if config.sandbox {
        orders::new(
            sandbox_token.clone(),
            config.account_id.clone(),
            "sandbox.tradier.com".into(),
            persistence.clone(),
        )
        .expect("Failed to create OrdersService")
    } else {
        orders::new(
            access_token.clone(),
            config.account_id.clone(),
            "api.tradier.com".into(),
            persistence.clone(),
        )
        .expect("Failed to create OrdersService")
    };
    let shutdown = Arc::new(AtomicBool::new(false));
    let mut symbols: HashSet<String> = HashSet::new();

    config.strategies.into_iter().for_each(|strategy| {
        symbols.extend(strategy.symbols.clone());
        let mut trading_service = trading::new(
            strategy.name.clone(),
            strategy.symbols.clone(),
            strategy.capital.clone(),
            market_data.clone(),
            historical_data.clone(),
            orders.clone(),
        );
        match trading_service.run() {
            Ok(_) => (),
            Err(e) => eprintln!("Error starting TradingService {}: {}", strategy.name, e),
        }
    });

    ...
}
```

It feels a bit archaic to have to hand-code this type of thing, which a dependency-injection library would do for us. On the other than, the manual rather than declarative style makes for complete transparency.

Ok, that’s it for the boring stuff. Next, backtesting.
