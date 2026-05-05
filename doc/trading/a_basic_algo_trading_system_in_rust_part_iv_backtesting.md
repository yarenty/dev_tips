---
title: A Basic Algo Trading System In Rust: Part IV: Backtesting
main_link: https://github.com/Paul-Folbrecht/algo-trading
keywords: [a-basic-algo-trading-system-in-rust-part-iv-backtesting, trading, data, system, backtesting, rust]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/trading/rust_algo_trading.md` by `article_split.py`. Heading: **A Basic Algo Trading System In Rust: Part IV: Backtesting**.

# A Basic Algo Trading System In Rust: Part IV: Backtesting

**Main link:** <https://github.com/Paul-Folbrecht/algo-trading>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#a-basic-algo-trading-system-in-rust-part-iv-backtesting` `#trading` `#data` `#system` `#backtesting` `#rust`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# A Basic Algo Trading System In Rust: Part IV: Backtesting
Paul Folbrecht
Rustaceans

Jul 22, 2024








I’m going to start this post off with a spoiler: It made money.

As previously stated, I built this algorithmic trading system mainly as an exercise, to explore the process of developing a non-trivial server application in Rust, seeking best processes at every step. Although I may fund it some day, using it is secondary.

Thus, I admit to being slightly surprised to discover, upon examining the first real backtest run, that it had generated alpha that beat the index (NASDAQ): A two-year return of ~62.9% vs. 53.5% for the same time period, trading AAPL and AMZN:

![img_5.png](../assets/img/trading/img_5.png/img/trading/img_5.png)
(P&L of $12,570 on $20,000 total capital.)

This is with a Bollinger Band mean-reversion strategy — about as simple as it gets.

(To note again, the system facilitates “pluggable” strategies — anything that can be code can be inserted.)

## Functionality
Backtesting is “the general method for seeing how well a strategy or model would have done after the fact. It assesses the viability of a trading strategy by discovering how it would play out using historical data. If backtesting works, traders and analysts may have the confidence to employ it going forward.”

In the real world, giving any trading model/system real money without having undergone thorough backtesting would be — foolish.

In this vein, I set out to extend the system to support backtesting for arbitrary ranges. A typical backtesting range is two years. From the config:

![img_6.png](../assets/img/trading/img_6.png/img/trading/img_6.png)

local.toml
While it would be very simple to make the end date configurable also, allowing for testing periods far in the past, I did not do that, as I wouldn’t test actual strats that way.

I decided that the backtester would be a separate application — in Rust parlance, another crate (and another package too, as we’ll see).

## Architectural Decision Record
This problem presented interesting questions regarding maximizing code reuse between real and backtesting trading.

I knew a few things right off the bat:

TradingService would be generalized to trade either “for real” or in backtesting mode. This would entail parameterization of current date.
I did not want to make multiple data fetches, which would be the most natural/easiest solution. Instead, I would fetch all data during initialization, and use new implementations of historical and market data services to use that canned data, rather than fetching for each day in the test range.
Though, as stated, there would be an entirely new executable (binary crate), I would use the same config structure, with extensions as needed. This means that, just like the real system, any number of strategies can be run at once.
We will see how, using the dependency injection scheme already devised, we can introduce new versions of outer services, using the same core trading code as the real/live system.

## Code
Here we can see the layout of the new crate (in Zed’s project view):

![img_7.png](../assets/img/trading/img_7.png/img/trading/img_7.png)

We’ve got several new service modules, a main, and a tests module. (As usual, we won’t be covering unit tests here, but check out the repo if you’re interested.)

Let’s go through the services one by one, then see how they’re tied together.

### BacktestOrderService

```rust

use domain::domain::*;
use services::orders::OrderService;
use std::sync::Arc;
use std::{collections::HashMap, sync::Mutex};

pub trait BacktestOrderService: OrderService {
    fn open_positions(&self) -> Vec<Position>;
    fn realized_pnl(&self) -> Vec<RealizedPnL>;
}

pub fn new() -> Arc<impl BacktestOrderService + Send + Sync> {
    Arc::new(implementation::BacktestOrders {
        positions: Arc::new(Mutex::new(HashMap::new())),
        pnl: Arc::new(Mutex::new(Vec::new())),
    })
}

mod implementation {
    use super::*;
    use services::orders::implementation::*;

    pub struct BacktestOrders {
        pub positions: Arc<Mutex<HashMap<String, Position>>>,
        pub pnl: Arc<Mutex<Vec<RealizedPnL>>>,
    }

    impl BacktestOrderService for BacktestOrders {
        fn open_positions(&self) -> Vec<Position> {
            self.positions
                .lock()
                .unwrap()
                .values()
                .filter(|p| p.quantity > 0)
                .cloned()
                .collect()
        }

        fn realized_pnl(&self) -> Vec<RealizedPnL> {
            self.pnl.lock().unwrap().clone()
        }
    }

    impl OrderService for BacktestOrders {
        fn create_order(&self, order: Order, strategy: String) -> Result<Order, String> {
            let position = position_from(&order, self.get_position(&order.symbol));
            self.update_position(&position);

            if order.side == Side::Sell {
                let pnl = calc_pnl(position, &order, strategy);
                self.pnl.lock().unwrap().push(pnl.clone());
                println!("Generated P&L: {:?}", pnl);
            }

            Ok(order)
        }

        fn get_position(&self, symbol: &str) -> Option<Position> {
            let positions = self.positions.lock().unwrap();
            positions.get(symbol).cloned()
        }

        fn update_position(&self, position: &Position) {
            self.positions
                .lock()
                .unwrap()
                .insert(position.symbol.clone(), position.clone());
        }
    }
}

```


We see something Rust-new (for this project) here: A trait that extends another. BacktestOrderService needs to be an OrderService, but add new functionality.

Basically, this implementation is a sort of mock. Instead of creating orders with the broker and storing orders, positions, and P&L in our Mongo database, it paper trades entirely, tracking positions and realized P&L for inspection after the backtest.

### BacktestHistoricalDataManager
This is the new artifact we need to manage historical data across the entire span of the backtesting range. It implements the existing HistoricalDataService interface, adding all() functionality to return the entire data set, needed by the corresponding market data abstraction.

```rust

use chrono::NaiveDate;
use domain::domain::Day;
use services::historical_data::HistoricalDataService;
use std::collections::HashMap;
use std::sync::Arc;

pub trait BacktestHistoricalDataManager: HistoricalDataService {
    fn all(&self) -> Arc<HashMap<String, Vec<Day>>>;
}

pub fn new(
    end: NaiveDate,
    range: i64,
    hist_data_range: i64,
    underlying: Arc<impl HistoricalDataService + 'static + Send + Sync>,
) -> Arc<impl BacktestHistoricalDataManager> {
    Arc::new(implementation::BacktestHistoricalData {
        end,
        range,
        hist_data_range,
        underlying,
    })
}

mod implementation {
    use super::*;

    pub struct BacktestHistoricalData<H: HistoricalDataService + 'static + Send + Sync> {
        pub end: NaiveDate,
        pub range: i64,
        pub hist_data_range: i64,
        pub underlying: Arc<H>,
    }

    impl<H: HistoricalDataService + 'static + Send + Sync> BacktestHistoricalDataManager
        for BacktestHistoricalData<H>
    {
        fn all(&self) -> Arc<HashMap<String, Vec<Day>>> {
            self.underlying.fetch(self.end).clone()
        }
    }

    impl<H: HistoricalDataService + 'static + Send + Sync> HistoricalDataService
        for BacktestHistoricalData<H>
    {
        fn fetch(&self, end: NaiveDate) -> Arc<HashMap<String, Vec<Day>>> {
            // end is some past trading day. We want to fetch from end - range - hist_data_range to end
            let start = end - chrono::Duration::days(self.hist_data_range);
            println!(
                "BacktestHistoricalData.fetch: fetching from {} to {}",
                start, end
            );
            let data = self
                .underlying
                .fetch(end)
                .iter()
                .map(|(symbol, days)| {
                    let data = days
                        .iter()
                        .filter(|day| day.date >= start && day.date <= end)
                        .cloned()
                        .collect();
                    (symbol.clone(), data)
                })
                .collect();
            Arc::new(data)
        }
    }
}

#[cfg(test)]
#[path = "./tests/backtest_historical_data_test.rs"]
mod backtest_historical_data_test;
#[path = "./tests/mock_historical_data_service.rs"]
mod mock_historical_data_service;
```


You can see that we aggregate an actual HistoricalDataService, using it to do the data fetch — there’ll be no copying code here.

Notice the data-based filtering going on in fetch: The function filters down the large data set to a single, hist_data_range bite.

This filtering implementation falls into a category of technical solutions academically classified as fugly. I did not want to do it. And, in fact, I first wrote this much more elegant solution, drawing on experience from my hedge fund days:


This solution makes use of a very efficient data structure: A single array, which can be sliced by translating dates into array indices.

That means constant time access of subsets, as opposed to the O(n) time complexity of the first example.

However, when my unit tests failed, I realized I could not readily make use of this solution: There were gaps in the data. Because weekends and holidays. Forgot about that! That’s just the way historical trading data is, I recalled.

To get around that, I’d need exchange calendar data. And that isn’t free, and isn’t easy to manage. For a system like this, I decided it would be overkill.

(And, even with such pearl-clutch-inducing inefficiencies, it’s still blazingly fast: As can be noted from the output screenshot above, a two-year run with a single strategy takes well under a tenth of a second. That includes the output, which is likely the majority of the CPU time.

Yeah, no VM warmup is nice.)

### BacktestMarketDataManager & BacktestMarketDataService
This file contains two related services: The former is the whole-backtest-context abstraction that encapsulates the entire market data set and handles creating date-specific instances of the latter, which is a new implementation of MarketDataService that uses this canned data (instead of the live websocket feed of the live one).

```rust

use crate::backtest_historical_data::BacktestHistoricalDataManager;
use chrono::{DateTime, Local, NaiveDate, NaiveTime};
use core::util::print_map;
use domain::domain::{Day, Quote};
use services::market_data::MarketDataService;
use std::{collections::HashMap, sync::Arc};

pub trait BacktestMarketDataManager {
    fn service_for_date(
        &self,
        date: NaiveDate,
    ) -> Result<Arc<impl MarketDataService + 'static + Send + Sync>, String>;
}

pub fn new(
    access_token: String,
    symbols: Vec<String>,
    backtest_range: i64,
    end: NaiveDate,
    backtest_historical_data: Arc<impl BacktestHistoricalDataManager + Send + Sync>,
) -> Arc<impl BacktestMarketDataManager> {
    // We need to turn a map of symbol->days into a map of date->quotes
    let history: Vec<Day> = backtest_historical_data
        .all()
        .values()
        .flatten()
        .cloned()
        .collect();
    println!("\n\nBacktestMarketDataManager: history:\n{:?}", history);

    let mut quotes: HashMap<NaiveDate, Vec<Quote>> = HashMap::new();
    for day in history {
        let date: DateTime<Local> = day
            .date
            .and_time(NaiveTime::from_hms_opt(0, 0, 0).unwrap())
            .and_local_timezone(Local)
            .earliest()
            .expect("Failed to convert date to datetime");
        quotes.entry(day.date).or_insert_with(Vec::new).push(Quote {
            symbol: day.symbol.expect("Missing symbol").clone(),
            bid: day.close,
            ask: day.close,
            biddate: date,
            askdate: date,
        })
    }
    print_map("Quotes", &quotes);

    Arc::new(implementation::BacktestMarketData { quotes })
}

mod implementation {
    use super::*;
    use chrono::NaiveDate;
    use crossbeam_channel::Receiver;
    use std::collections::HashMap;

    pub struct BacktestMarketData {
        pub quotes: HashMap<NaiveDate, Vec<Quote>>,
    }

    impl BacktestMarketDataManager for BacktestMarketData {
        fn service_for_date(
            &self,
            date: NaiveDate,
        ) -> Result<Arc<impl MarketDataService + 'static + Send + Sync>, String> {
            match self.quotes.get(&date) {
                Some(quotes) => Ok(Arc::new(BacktestMarketDataService {
                    quotes: quotes.clone(),
                })),
                None => Err(format!("No quotes for date {}", date)),
            }
        }
    }

    pub struct BacktestMarketDataService {
        quotes: Vec<Quote>,
    }

    impl MarketDataService for BacktestMarketDataService {
        fn init(
            &self,
            shutdown: Arc<std::sync::atomic::AtomicBool>,
            symbols: Vec<String>,
        ) -> Result<std::thread::JoinHandle<()>, String> {
            Err("Please don't call this method".to_string())
        }

        fn subscribe(&self) -> Result<Receiver<Quote>, String> {
            let (sender, receiver) = crossbeam_channel::unbounded();
            self.quotes
                .iter()
                .for_each(|quote| match sender.send(quote.clone()) {
                    Ok(_) => (),
                    Err(e) => eprintln!("Error sending quote to subscriber: {}", e),
                });
            Ok(receiver)
        }

        fn unsubscribe(&self, subscriber: Receiver<Quote>) -> Result<(), String> {
            Ok(())
        }
    }
}

#[cfg(test)]
#[path = "./tests/backtest_market_data_manager_test.rs"]
mod backtest_market_data_manager_test;
#[path = "./tests/mock_historical_data_service.rs"]
mod mock_historical_data_service;
```

One interesting bit: The MarketDataService subscription mechanism is based on channels. The live service monitors its broker-data websocket, firing quotes as they arrive to all subscribers. Here, we have all the data at the outset: On subscribe, we simply create the channel and fire them all immediately.

![img_8.png](../assets/img/trading/img_8.png/img/trading/img_8.png)
Voila.

(Please see util.rs for the utility functions used above and elsewhere.)

## BacktestService
We’re almost through the new code, and now we’ll see the new services coming together.

In the interest of shaving a bit of length, I’m going to keep this gist to the guts, the run function:

```rust
        // - For each date in range:
        //   - Construct BacktestMarketDataService from MarketDataManager data
        //   - run() strategies - will subscribe to MarketDataService and be fed quotes
        fn run(&self) -> Result<(), String> {
            let start = self.end - chrono::Duration::days(self.backtest_range);
            println!("Running backtest from {} to {}", start, self.end);

            for i in 0..=self.backtest_range {
                let date = start + chrono::Duration::days(i);

                println!("\nRunning for {}", date);
                match self.market_data_manager.service_for_date(date) {
                    Ok(market_data) => {
                        self.strategies.clone().into_iter().for_each(|strategy| {
                            let mut trading_service = trading::new(
                                date,
                                strategy.name.clone(),
                                strategy.symbols.clone(),
                                strategy.capital.clone(),
                                market_data.clone(),
                                self.historical_data.clone(),
                                self.orders.clone(),
                            );

                            match trading_service.run() {
                                Ok(_) => {
                                    println!(
                                        "Strategy '{}' ran successfully for {}",
                                        strategy.name, date
                                    );
                                    trading_service
                                        .shutdown()
                                        .expect("Unexpected error shutting down trading_service");
                                }
                                Err(e) => {
                                    eprintln!(
                                        "Error starting TradingService {}: {}",
                                        strategy.name, e
                                    )
                                }
                            }
                        });
                    }
                    Err(_) => {
                        eprintln!("Skipping {} - no data (weekend or holiday)", date);
                        continue;
                    }
                }
            }

            Ok(())
        }
```

The comment at the top explains what’s going on, and it’s pretty simple — although, occasionally, Rust does feel a bit verbose.

Another detail worth noting is the shutdown of the trading services. Writing the backtest motivated me to clean up a couple loose ends in the system, such as full, clean shutdown.

Remember that each TradingService has an internal thread, to decouple and parallelize its processing. The shutdown:

![img_9.png](../assets/img/trading/img_9.png/img/trading/img_9.png)

(A further improvement to the system, for efficiency, would be to either keep these threads alive between ‘days’ or use a thread pool. This would net very little gain in practice, however.)

## Main
Here we are at the end: main.rs:

```rust

use app_config::app_config::AppConfig;
use backtest_orders::BacktestOrderService;
use backtest_service::BacktestService;
use chrono::Local;
use core::util::time;
use itertools::Itertools;
use services::historical_data;

mod backtest_historical_data;
mod backtest_market_data_manager;
mod backtest_orders;
mod backtest_service;

fn main() {
    let config = AppConfig::new().expect("Could not load config");
    println!("Config:\n{:?}", config);

    let end = Local::now().naive_local().date();
    let symbols = config.all_symbols();

    let historical_data = historical_data::new(
        config.access_token.clone(),
        symbols.clone(),
        config.backtest_range + config.hist_data_range,
        end,
    );

    let backtest_historical_data = backtest_historical_data::new(
        end,
        config.backtest_range,
        config.hist_data_range,
        historical_data,
    );

    let backtest_market_data_manager = backtest_market_data_manager::new(
        config.access_token.clone(),
        symbols.clone(),
        config.backtest_range,
        end,
        backtest_historical_data.clone(),
    );

    let orders = backtest_orders::new();
    let backtest_service = backtest_service::new(
        end,
        config.backtest_range,
        backtest_historical_data.clone(),
        backtest_market_data_manager,
        orders.clone(),
        config.strategies.clone(),
    );

    time("backtest_service.run()", || match backtest_service.run() {
        Ok(_) => {
            let pnl = orders.realized_pnl();
            println!(
                "\nBacktest completed successfully\n\nOpen positions:\n{:?}\n\nRealized P&L:\n{:?}\n\nTotal P&L: {}\n",
                orders.open_positions().iter().format("\n"),
                pnl.iter().format("\n"),
                pnl.iter().map(|pnl| pnl.pnl).sum::<f64>());
        }
        Err(e) => eprintln!("Backtest failed: {}", e),
    })
}
```

It’s simplicity itself: Construct the services, tie them together, run, and produce the output.

And — we are done, with this piece.

I want to call out a couple of the deliberate limitations of the system now:

We never increase a position, as we always buy the max amount allowed by the associated configured capital. Thus, only single price/cost basis is captured, which (bonus) makes the realized P&L calc very simple
If we enter and exit a position in the same day, the position’s cost basis is never updated from broker. It is initted from the quote (ask) that created the trading signal; since that is unlikely to be identical to the resulting order’s average fill price, the calc’d realized P&L will have a slight error.

Next: Containerization and Cloud deployment. (We don’t want to stop trading when we close our laptop.)


Reminder: The repo is here.


https://github.com/Paul-Folbrecht/algo-trading
