---
title: A Basic Algo Trading System In Rust: Part I
main_link: 
keywords: [a-basic-algo-trading-system-in-rust-part-i, rust, market, basic]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

> Auto-split from `doc/trading/rust_algo_trading.md` by `article_split.py`. Heading: **A Basic Algo Trading System In Rust: Part I**.

# A Basic Algo Trading System In Rust: Part I

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[a_basic_algo_trading_system_in_rust_part_iv_backtesting]] — A Basic Algo Trading System In Rust: Part IV: Backtesting _(score 24.9)_
- [[rust_algo_trading]] — Algo trading _(score 17.1)_
- [[liquid]] — Liquid _(score 13.1)_
- [[rtic]] — RTIC _(score 13.1)_
- [[scope]] — Scope _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#a-basic-algo-trading-system-in-rust-part-i` `#trading` `#rust` `#system` `#data` `#market`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# A Basic Algo Trading System In Rust: Part I
Paul Folbrecht
Rustaceans

Jun 8, 2024







## Introduction
Writing a personal algorithmic trading system is a fantastic way to make substantial amounts of money with minimal effort.

Ha! If only that were so. No, because of the efficiency of equity and other liquid markets, etc., easy money is not to be had.

However, writing such a system for fun and as a learning exercise (both technical and trading) is certainly worthwhile. This series will demonstrate the creation of a such a system, simple but fully-functional, in Rust.

A query is prompted: Why Rust? Python is, by far, the most popular programming language for algo traders. With Python & numpy, one can be off to the races in a hurry.

But a language like Rust still has much appeal:

It is blazingly fast, natively (not just with libs written in C). There are contexts where this matters.
It is far more flexible — in a sense, infinitely so, compared to programming with a glue language using canned libraries.
It’s much more robust, even than Python, and certainly compared to any other systems-level language
While the system presented here is ‘toy’ in the sense that the one simple strategy presented is unlikely to generate alpha, except by coincidence or with highly-educated choosing of names, the system is also a reasonably complete framework, with pluggable trading strategies. Thus, anything is possible.

The piece is more about Rust application architecture and idiomatic programming than it is about the domain.

Complete source code is available.

## Functionality
I’m going to assume little-to-no understanding of algorithmic trading. The basics functional requirements of such a system are as follows:

Gather live and historical market data
Feed that data to a trading strategy which can determine to buy or sell securities
[Optional: Run proposed trades through some sort of risk system. We’ll consider risk assessment out-of-scope here — something to be decided non-programmically.]
Place and track orders, and positions
In 2024, there are a number of very good commercial trading API offerings. I chose to use the Tradier system for the following reasons:

Good reputation for completeness of functionality, reliability, and customer service
It provides both live and historical data, and full order/position functionality
Reasonable pricing
Architectural Decision Record
Having had good success with so-called onion architecture (most recently specifically with the ZIO Service Pattern in Scala, incidentally), I sought to discover if this was an appropriate architecture for Rust applications. It turns out that it is, with certain caveats specific to this language.

In that vein, I created a workspace-based project, with several libraries with dependencies all pointing in the right direction: from services to domain to core. That is, the most critical part of the application, domain, where business logic lives, has no dependencies on I/O or technical implementation concerns.
![img_3.png](../assets/img/trading/img_3.png/img/trading/img_3.png)

## Services
Starting from a clean, rusty slate, as I set out to do, the simplest solution is bare functions. But we need to be able to do at least one other thing: Mock implementations for unit testing. With that in mind, using some mechanism to group related functions together and change the implementation at will seems like a good idea.

Thankfully, Rust is not an object-oriented language; rather than bundling the rather disparate elements of interface, encapsulation, and dispatch/polymorphism together, it keeps these concerns distinct and allows one to compose them appropriately, as desired.

The interface mechanism in Rust is the trait: Traits groups related sets of functions together which can be implemented in various ways.

With traits and struct implementations, we can implement a simple but entirely adequate mechanism of dependency injection, and thus make our code unit-testable.

There are essentially two ways to use traits to achieve pluggable implementations in Rust: Static (compile-time) binding — impl Trait — and dynamic binding — the dyn Trait mechanism. I chose the former, for reasons I won’t get in to here, but both are viable, and the slight performance hit of dynamic binding would not be an issue for service-level interaction.

## Concurrency
It is advantageous for almost all nontrivial applications to make use of some sort of parallelism mechanism. Here, we know we are going to have several disparate concerns that should ideally run in parallel:

Responding to market events
Making trading decisions
Placing or editing orders
With that in mind, I made the following decisions:

A single process: There is no reason here to incur the overhead of both additional structural complexity and poorer performance that breaking the system into microservices would incur. Rather, we will use a clean logical dependency structure to keep application portions appropriately isolated
No async: This type of application will never be in the position of handling a large number of concurrent I/O events. Thus, we do not need to multiplex threads onto tasks, and going async has no real purpose here. (Async Rust is, in a sense, a beast unto itself, and best avoided unless you need what it buys you — IMHO.)
Channels rather than ZeroMQ or another in process messaging mechanism. Rust’s built-in channels are an excellent mechanism for decoupling in-process components; there is no reason in Rust to use a third-party messaging library for this type of use case. Doing so we needlessly increase complexity And likely decrease performance.
Getting into the weeds, I chose tungstenite over ezsockets for websocket support, crossbeam over native channels because they add useful functionality at no real cost, and the ubiquitous reqwest library for HTTP, along with its pal serde for serialization.
Code


![img_2.png](../assets/img/trading/img_2.png/img/trading/img_2.png)
Though this is an exercise in building a real-world, relatively complex system in Rust, some elements of true production-grade software are missing:

Use of a proper ADT error system rather than ‘String’
Clean shutdown [Update: Handled in a later iteration.]
With that prelude, it’s time to get into the code — parts of it, that is. We won’t be covering here utilities (core), unit tests, and other less interesting bits.

We’ll start with fn main.

```rust
fn main() {
    let config = AppConfig::new().expect("Could not load config");
    println!("Config:\n{:?}", config);
    let args: Vec<String> = std::env::args().collect();
    let access_token = &args[1];
    let market_data_service = market_data::new(access_token.to_string());
    let historical_data_service = historical_data::new(access_token.to_string());
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
        match trading_service.run() {
            Ok(_) => (),
            Err(e) => eprintln!("Error starting TradingService {}: {}", strategy.name, e),
        }
    });

    let handle = market_data_service.init(shutdown, symbols.into_iter().collect());
    match handle.unwrap().join() {
        Ok(_) => println!("MarketDataService thread exited successfully"),
        Err(e) => eprintln!("Error joining MarketDataService thread: {:?}", e),
    }
}
```

main loads the application’s config, which specifies the trading strategies to be instantiated along with their parameters, then instantiates them with the dependent services.

Note that a set of symbols to be monitored by market data is constructed from the parameters of all configured strategies. This is done for efficiency: We want to use a single websocket for all market data events.

The config, as noted, supports n trading strategies:

```yaml
[[strategies]]
name = "mean-reversion"
symbols = ["SPY", "AAPL"]

# [[strategies]]
# name = "pairs"
# symbols = []
```

The system in its present state supports only mean-reversion as a strategy, but is set up to configure and create any number of any type of trading strategy implementation.

Market data sources are, in a sense, the heart of any trading system. No live data, no trading.

```rust
pub trait MarketDataService {
    fn init(
        &self,
        shutdown: Arc<AtomicBool>,
        symbols: Vec<String>,
    ) -> Result<JoinHandle<()>, String>;
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

mod implementation {
    use super::*;

    #[derive(Deserialize)]
    struct AuthResponse {
        stream: Stream,
    }

    #[derive(Deserialize)]
    struct Stream {
        sessionid: String,
    }

    pub struct MarketData {
        pub access_token: String,
        pub socket: Option<WebSocket<MaybeTlsStream<TcpStream>>>,
        pub symbols: HashSet<String>,
        pub subscribers: Arc<Mutex<Vec<(Sender<Quote>, Receiver<Quote>)>>>,
    }

    impl MarketDataService for MarketData {
        fn init(
            &self,
            shutdown: Arc<AtomicBool>,
            symbols: Vec<String>,
        ) -> Result<JoinHandle<()>, String> {
            let response = authenticate(&self.access_token).expect("Error authenticating");
            let session_id = &response.stream.sessionid;
            let symbols_json = serde_json::to_string(&symbols).expect("Error serializing symbols");

            match connect("wss://ws.tradier.com/v1/markets/events") {
                Ok((mut socket, _)) => {
                    let message = format!(
                        "{{\"symbols\": {}, \"sessionid\": \"{}\", \"filter\": [\"quote\"], \"linebreak\": true}}",
                        symbols_json,
                        session_id
                    );
                    socket
                        .send(Message::Text(message))
                        .expect("Error sending message");

                    let subscribers = self.subscribers.clone();
                    let handle = std::thread::spawn(move || {
                        while !shutdown.load(std::sync::atomic::Ordering::Relaxed) {
                            let msg = socket
                                .read()
                                .expect("Error reading message")
                                .into_text()
                                .expect("Error converting message to text");
                            let quote = serde_json::from_str::<Quote>(msg.as_str())
                                .expect("Error parsing JSON");
                            println!("MarketDataService received quote: {:?}", quote);
                            for subscriber in subscribers.lock().unwrap().iter() {
                                match subscriber.0.send(quote.clone()) {
                                    Ok(_) => (),
                                    Err(e) => eprintln!("Error sending quote to subscriber: {}", e),
                                }
                            }
                        }
                    });

                    Ok(handle)
                }

                Err(e) => Err(e.to_string()),
            }
        }

        fn subscribe(&self) -> Result<Receiver<Quote>, String> {
            let (sender, receiver) = unbounded();
            let subscriber = receiver.clone();
            self.subscribers
                .lock()
                .and_then(|mut s| Ok(s.push((sender, receiver))))
                .map_err(|e| e.to_string())
                .and_then(|_| Ok(subscriber))
        }

        fn unsubscribe(&self, subscriber: Receiver<Quote>) -> Result<(), String> {
            match self.subscribers.lock() {
                Ok(mut guard) => {
                    let subscribers: &mut Vec<(Sender<Quote>, Receiver<Quote>)> = &mut *guard;
                    if let Some(index) = subscribers
                        .iter()
                        .position(|(_, r)| std::ptr::eq(r, &subscriber))
                    {
                        subscribers.remove(index);
                        Ok(())
                    } else {
                        return Err("No such subscriber found".to_string());
                    }
                }
                Err(e) => return Err(e.to_string()),
            }
        }
    }

    fn authenticate(access_token: &str) -> reqwest::Result<AuthResponse> {
        match reqwest::blocking::Client::new()
            .post("https://api.tradier.com/v1/markets/events/session")
            .header(AUTHORIZATION, format!("Bearer {}", access_token))
            .header(ACCEPT, "application/json")
            .header(CONTENT_LENGTH, "0")
            .send()?
            .json::<AuthResponse>()
        {
            Ok(r) => Ok(r),
            Err(e) => {
                eprintln!("Error: {}", e);
                Err(e)
            }
        }
    }
}

#[cfg(test)]
#[path = "./tests/market_data_test.rs"]
mod market_data_test;

```

This gist demonstrates the pattern for services I followed throughout the app (some parts of which are unique, AFAIK):

A trait for the interface (because I’m not using dynamic binding with trait objects, I’m able to use generic types in these interfaces, though we don’t see that here)
An fn new() constructor that returns impl Trait wrapped by an Arc — the Arc is necessary to allow shared access (among threads) to the service
An implementation by struct hidden in a submodule; this is not necessary for visibility reasons (of course, non-pub members of the module are not visible outside it, except to submodules); I did this for organizational purposes only
Unit tests in a separate file using the #path macro (contrary to the Rust norm of putting tests in the same file as the production code —sorry but that’s just a bad norm)
Implementation elements:

subscribe() registers a subscriber by creating an unbounded channel.
init() connects a websocket to the Tradier market data streaming endpoint. The system collected symbols from all registered trading strategies (above) — this set is sent to the socket to register for events. Note also that we are filtering on “quote” events. See the Tradier API Docs for details.
init() then spawns a thread to listen to the websocket and send quotes to subscribers
The subscribers collection is guarded by a mutex. Unlike legacy languages, mutexes in Rust not only guard data but also own it, making it impossible to mutate without acquiring a lock.
Finally, note that you must acquire a Tradier API access token in order to use the system. This is passed as a command-line argument to the process.

[Update: Secrets are now handled by the environment.]

Historical data is also necessary for virtually every trading strategy.

At first blush, one might assume that live and historical data should be abstracted behind a common interface. But this is not accurate — the data provided is different, which is reflected by our structs, which mirror Tradier (and other) API responses:

```rust
#[derive(Deserialize, Debug, Clone)]
pub struct Quote {
    pub symbol: String,
    pub bid: f64,
    pub ask: f64,
    #[serde(with = "tradier_date_time_format")]
    pub biddate: DateTime<Local>,
    #[serde(with = "tradier_date_time_format")]
    pub askdate: DateTime<Local>,
}

#[derive(Deserialize, Debug)]
pub struct History {
    pub day: Vec<Day>,
}

#[derive(Deserialize, Debug)]
pub struct Day {
    #[serde(with = "tradier_date_format")]
    pub date: NaiveDate,
    pub open: f64,
    pub high: f64,
    pub low: f64,
    pub close: f64,
    pub volume: i64,
}

```

So, “live” and “historical” quotes are fundamentally different.

(But, “live” doesn’t necessarily always mean literally “live”: When we get to backtesting, “live” quotes will actually be historical.)

Here’s the service code:


This is simpler than the MarketDataService, as it uses a simple HTTP Get request rather than a websocket (because this isn’t live data).

Now we’re onto the good stuff — TradingService:

```rust
pub trait TradingService {
    fn run(&mut self) -> Result<(), String>;
}

pub fn new<'market_data>(
    strategy_name: String,
    symbols: &'market_data Vec<String>,
    market_data_service: Arc<impl MarketDataService + 'market_data + Send + Sync>,
    historical_data_service: Arc<impl HistoricalDataService + 'market_data + Send + Sync>,
) -> impl TradingService + 'market_data {
    implementation::Trading {
        strategy_name,
        symbols,
        market_data_service,
        historical_data_service,
        thread_handle: None,
    }
}

mod implementation {
    use super::*;
    use chrono::Local;
    use domain::domain::SymbolData;
    use std::collections::HashMap;

    pub struct Trading<
        'market_data,
        M: MarketDataService + Send + Sync,
        H: HistoricalDataService + Send + Sync,
    > {
        pub strategy_name: String,
        pub symbols: &'market_data Vec<String>,
        pub market_data_service: Arc<M>,
        pub historical_data_service: Arc<H>,
        pub thread_handle: Option<std::thread::JoinHandle<()>>,
    }

    impl<
            'market_data,
            M: MarketDataService + Send + Sync,
            H: HistoricalDataService + Send + Sync,
        > TradingService for Trading<'market_data, M, H>
    {
        fn run(&mut self) -> Result<(), String> {
            println!(
                "Running TradingService with strategy: {:?}",
                self.strategy_name
            );
            let symbol_data = load_history(self.symbols, self.historical_data_service.clone());

            match self.market_data_service.subscribe() {
                Ok(rx) => {
                    println!("TradingService subscribed to MarketDataService");
                    let strategy = Strategy::new(&self.strategy_name, self.symbols.clone());
                    self.thread_handle = Some(std::thread::spawn(move || loop {
                        match rx.recv() {
                            Ok(quote) => {
                                println!("TradingService received quote:\n{:?}", quote);
                                if let Some(symbol_data) = symbol_data.get(&quote.symbol) {
                                    strategy.handle(&quote, symbol_data);
                                }
                            }
                            Err(e) => {
                                eprintln!("Error on receive!: {}", e);
                            }
                        }
                    }));
                }

                Err(e) => return Err(format!("Failed to subscribe to MarketDataService: {}", e)),
            }
            Ok(())
        }
    }

    fn load_history<'market_data>(
        symbols: &'market_data Vec<String>,
        historical_data_service: Arc<impl HistoricalDataService + 'market_data>,
    ) -> HashMap<String, SymbolData> {
        symbols
            .iter()
            .map(|symbol| -> (String, SymbolData) {
                let end = Local::now().naive_local().date();
                let start = end - chrono::Duration::days(20);
                println!("Loading history for {} from {} to {}", symbol, start, end);
                let query: Result<Vec<domain::domain::Day>, reqwest::Error> =
                    historical_data_service
                        .fetch(symbol, start, end)
                        .map(|h| h.day);
                match query {
                    Ok(history) => {
                        let sum = history.iter().map(|day| day.close).sum::<f64>();
                        let len = history.len() as f64;
                        let mean = sum / len;
                        let variance = history
                            .iter()
                            .map(|quote| (quote.close - mean).powi(2))
                            .sum::<f64>()
                            / len;
                        let std_dev = variance.sqrt();
                        let data = SymbolData {
                            symbol: symbol.clone(),
                            history,
                            mean,
                            std_dev,
                        };
                        println!("Loaded history for {}: {:?}", symbol, data);
                        (symbol.to_owned(), data)
                    }
                    Err(e) => panic!("Can't load history for {}: {}", symbol, e),
                }
            })
            .into_iter()
            .collect()
    }
}

#[cfg(test)]
#[path = "./../tests/trading_test.rs"]
mod trading_test;
```

With this gist, the Rust Elephant has entered the room: Lifetimes. We see one here: ‘market_data. Even though this piece isn’t a Rust tutorial per se, we have to spend some time on this.

I will share the insight that was the aha! moment for me regarding Rust lifetimes:

Lifetimes tie references together, indicating they point to the same piece of memory.

That is what they do. That is what they are for, and all they are for. Understand this, and the mysteries of the borrow checker will begin to unravel.

So, here we have a specified lifetime. The compiler will not allow it to be elided because it cannot be inferred — it is used in struct member references.

What we are specifying with this lifetime is that all such references are tied to the set of symbols. That’s it. That’s the data the lifetime refers to.

(This lifetime could have certainly been ‘static, incidentally. I wanted to illustrate usage of a non-static lifetime, and also the idiom of naming them appropriately.)

There are two main functions here:

load_history loads the last 20 days of price history (extremely boorish of me to have that figure hard-coded, I realize, but it’s kind of a standard for the Bollinger Band mean-reversion we’ll be getting to), calculates the basic statistics (mean and standard deviation) we’ll need for the algo, and stores that data.
run processes quotes from the market and passes them to the attached Strategy, which will make trading decisions.
Finally, we get to the thingy that does — or will — actually do some trading:

```rust
#[derive(Debug, Clone)]
pub enum Strategy {
    MeanReversion { symbols: Vec<String> },
}

pub trait StrategyHandler {
    fn handle(&self, quote: &Quote, data: &SymbolData);
}

impl Strategy {
    pub fn new(name: &str, symbols: Vec<String>) -> Self {
        match name {
            "mean-reversion" => Strategy::MeanReversion { symbols },
            _ => panic!("Unknown strategy: {}", name),
        }
    }
}

impl StrategyHandler for Strategy {
    fn handle(&self, quote: &Quote, data: &SymbolData) {
        match self {
            Strategy::MeanReversion { symbols } => {
                if symbols.contains(&quote.symbol) {
                    println!("MeanReversionStrategy handling quote: {:?}", quote);
                    println!(
                        "Px: {}; Mean: {}; Std Dev: {}",
                        quote.ask, data.mean, data.std_dev
                    );
                    println!(
                        "quote.ask: {}; (data.mean - 2.0 * data.std_dev): {}",
                        quote.ask,
                        data.mean - 2.0 * data.std_dev
                    );

                    let buy = quote.ask < data.mean - 2.0 * data.std_dev;
                    if buy {
                        println!("***Buy signal for {}***", quote.symbol);
                    } else {
                        println!("No signal for {}", quote.symbol);
                    }
                }
            }
        }
    }
}

```

The algorithm on display here is a variant of mean-reversion inspired by Bollinger Bands. We see that the decision to enter long is based on this simple equation (in English): Is the (ask) price less than the mean minus two standard deviations?

Instructions for running, from the readme:

```log
# algo-trading

Algorithmic trading strategies and backtesting in Rust.

To obtain an access token, create an account at Tradier, then go to [this page](https://documentation.tradier.com/brokerage-api/oauth/access-token).

## Configuration

At present, only a very simple Bolinger Bands strategy is implemented.

Modify either `config\default.toml` or `config\local.toml` to configure symbols. Example:

```
[[strategies]]
name = "mean-reversion"
symbols = ["SPY", "AAPL"]
```

## Building

`cargo build`

## Running

`cargo run <access-token>`
```

Finally, here is a screenshot of system startup (with my access token blurred):


Even with only two (liquid) names, quotes will fly by at blinding speed at all times during market hours.

And, hey, look at that: AAPL actually got reasonably close to entry range.

Next

As is clear from the above, our singular trading strategy isn’t doing anything other than making a buy decision. We have the following left:
![img_1.png](../assets/img/trading/img_1.png/img/trading/img_1.png)
Making sell decisions
Generating Tradier API orders
Storing orders & positions locally (mongodb)
Recing local data with Tradier on startup
Additional, and more sophisticated, trading strategies, starting with using volume-weighted history
The repository is here.

UPDATE: Part II published.












## A Basic Algo Trading System In Rust: Part II
Paul Folbrecht
Rustaceans
Jun 27, 2024




We’re back for the next phase of this little algo-trading system — and by the end of this piece, we’ll be trading.

## Functionality
At the end of Part I, we had a system that could stream live market data, obtain historical price data, and a framework for running arbitrary trading strategies.

With this stage, we’ll accomplish the following:

- Full generation of signals from the mean-reversion strategy
- Translation of signals into possible orders
- Broker order creation (and execution) — only market orders, and no shorting (short-selling comes with unlimited theoretical risk and we don’t want to be one of those hobby traders who loses his life savings)
- Persistence of orders & positions in a private database (MongoDB)
- A “sandbox” mode for paper trading (necessary both for backtesting and so that so running unit tests doesn’t lead to bankruptcy)


## Architectural Decision Record
Almost all of the true “architectural” decisions were made in Part I, but there were a couple things to consider in this expansion of functionality.

Most pertinent was which new areas should be synchronous (with trading threads) or asynchronous.

We decouple things asynchronously for essentially two reasons:

- To isolate components for fault-tolerance — an async component can possibly die without bringing down the system
- To improve performance/throughput

Always think about what should be decoupled — it adds complexity. Decoupling increases resilience, but if all pieces are required for the system to function, there’s no win there. Breaking things apart simply increases complexity and creates more failure points.


The question I faced here was whether or not to decouple the following two tasks from trading threads:

- Order Creation
- Local (Non-Broker) Persistence

### Order Creation
Order creation has to be synchronous, because it is necessary to be certain an order has gone to the street before proceeding to possibly create further orders. The system cannot function correctly otherwise.

### Persistence
Local persistence, on the other hand, is a non-critical activity. In commercial trading systems I’ve architected, I’ve always ensured that trading could proceed even in the case of the local persistence layer being unavailable. That should generally be the rule.

## Code
I’m going to begin with sharing a part of core/utility code.

As I began to write new HTTP requests, I realized there were a few aspects I wanted to abstract over and encapsulate:

- Headers
- Retry with exponential backoff
- Response Deserialization

This did the trick:

```rust
pub fn get<T: DeserializeOwned>(url: &str, token: &str) -> Result<T, String> {
    let op = || {
        reqwest::blocking::Client::new()
            .get(url)
            .headers(headers(token))
            .send()
            .map_err(backoff::Error::transient)
    };

    call(op)
}

pub fn post<T: DeserializeOwned>(url: &str, token: &str, body: String) -> Result<T, String> {
    let op = || {
        reqwest::blocking::Client::new()
            .post(url)
            .headers(headers(token))
            .body(body.clone())
            .send()
            .map_err(backoff::Error::transient)
    };

    call(op)
}

fn headers(token: &str) -> HeaderMap<HeaderValue> {
    let mut headers = HeaderMap::new();
    headers.insert(
        AUTHORIZATION,
        HeaderValue::from_str(&format!("Bearer {}", token)).unwrap(),
    );
    headers.insert(ACCEPT, HeaderValue::from_static("application/json"));
    headers.insert(CONTENT_LENGTH, HeaderValue::from_static("0"));
    headers
}

fn backoff() -> ExponentialBackoff {
    ExponentialBackoff {
        initial_interval: std::time::Duration::from_millis(100),
        max_interval: std::time::Duration::from_secs(2),
        max_elapsed_time: Some(std::time::Duration::from_secs(5)),
        ..ExponentialBackoff::default()
    }
}

fn call<T, F, E>(op: F) -> Result<T, String>
where
    T: DeserializeOwned,
    F: FnMut() -> Result<Response, Error<E>>,
    E: std::fmt::Display,
{
    retry(backoff(), op)
        .map_err::<String, _>(|e| e.to_string())
        .and_then(|r| {
            r.json::<T>()
                .map_err::<String, _>(|e| format!("Could not parse response body: {}", e))
        })
}


```


ExponentialBackoff is from the backoff crate. Again and again, using Rust, I am impressed by how much solid functionality exists in the library ecosystem, and how easy it is to use.

(This Scala programmer felt very much at home with this type of trait-based abstraction, incidentally.)

Next, we have the new OrderService. We’ll take this in pieces, as it’s a bit large already.
```rust

pub trait OrderService {
    fn create_order(&self, order: Order) -> Result<Order, String>;
    fn get_position(&self, symbol: &str) -> Option<Position>;
    fn update_position(&self, position: &Position);
}

pub fn new(
    access_token: String,
    account_id: String,
    base_url: String,
    persistence: Arc<impl PersistenceService + Send + Sync>,
) -> Result<Arc<impl OrderService>, String> {
    let positions = implementation::read_positions(&base_url, &access_token, &account_id)?;
    println!("Read positions from broker: {:?}", positions);
    implementation::update_local_positions(persistence.clone(), &positions)?;

    Ok(Arc::new(implementation::Orders {
        access_token,
        account_id,
        base_url,
        persistence,
        positions: Arc::new(Mutex::new(positions)),
    }))
}

mod implementation {
    use super::*;
    use chrono::Local;
    use serde::Deserialize;

    pub struct Orders<P: PersistenceService + Send + Sync> {
        pub access_token: String,
        pub account_id: String,
        pub base_url: String,
        pub persistence: Arc<P>,
        pub positions: Arc<Mutex<HashMap<String, Position>>>,
    }

    #[derive(Deserialize)]
    struct OrderResponse {
        order: OrderData,
    }

    #[derive(Deserialize)]
    struct OrderData {
        id: i64,
        status: String,
    }

    impl<P: PersistenceService + Send + Sync> OrderService for Orders<P> {
      ...
    }

    ...
    
    #[derive(Deserialize)]
    struct PositionResponse {
        positions: Positions,
    }

    #[derive(Deserialize)]
    struct Positions {
        position: Vec<TradierPosition>,
    }

    pub fn read_positions(
        base_url: &str,
        access_token: &str,
        account_id: &str,
    ) -> Result<HashMap<String, Position>, String> {
        let url = format!("https://{}/v1/accounts/{}/positions", base_url, account_id);
        println!("url: {}", url);
        let response = get::<PositionResponse>(&url, &access_token);

        match response {
            Ok(response) => {
                let mut positions = HashMap::new();
                response
                    .positions
                    .position
                    .into_iter()
                    .for_each(|position| {
                        positions.insert(position.symbol.clone(), position.into());
                    });
                Ok(positions)
            }
            Err(e) => {
                eprintln!("Error reading positions - probably there are < 2: {}", e);
                Ok(HashMap::new())
            }
        }
    }

    pub fn update_local_positions(
        persistence: Arc<impl PersistenceService>,
        positions: &HashMap<String, Position>,
    ) -> Result<(), String> {
        // In the future, we may rec, but for now we'll update all positions from the source of truth
        persistence.drop_positions()?;
        positions
            .values()
            .map(|position| persistence.write(Box::new(position.clone())))
            .collect()
    }
}

```


Above we see the definition of the service along with the code to initialize it.

The methods shown are what is required for a minimally-viable product: We need to be able to create orders, and trading logic needs access to open positions.

On startup the system reads positions from the broker, and, for now, simply overwrites the local store with them — the broker is the Source of Truth.

Note that there is a translation from the broker/HTTP-specific artifacts shown above and the domain representation, done in the idiomatic Rust way of implementing the From trait:

```rust

#[derive(Deserialize)]
pub struct TradierPosition {
    pub id: i64,
    pub symbol: String,
    pub quantity: f64,
    pub cost_basis: f64,
    #[serde(with = "tradier_string_date_time_format")]
    pub date_acquired: DateTime<Local>,
}

#[derive(Serialize, Deserialize, Debug, Clone)]
pub struct Position {
    pub broker_id: Option<i64>,
    pub symbol: String,
    // Integer quantity as we'll only trade equities
    pub quantity: i64,
    pub cost_basis: f64,
    #[serde(with = "tradier_date_time_format")]
    pub date: DateTime<Local>,
}

impl From<TradierPosition> for Position {
    fn from(tp: TradierPosition) -> Self {
        Position {
            broker_id: Some(tp.id),
            symbol: tp.symbol,
            quantity: tp.quantity as i64,
            cost_basis: tp.cost_basis,
            date: tp.date_acquired,
        }
    }
}

impl Position {
    pub fn with_id(&self, id: i64) -> Self {
        Position {
            broker_id: Some(id),
            ..self.clone()
        }
    }

    pub fn with_cost_basis(&self, cost_basis: f64) -> Self {
        Position {
            cost_basis,
            ..self.clone()
        }
    }
}

```

Because implementing From gives you Into as well, we can call into() on a TradierPosition, and the compiler figures out, from type signatures alone, what we want to convert to — cool.

(The serde artifacts here are for the BSON serialization in the persistence layer.)

The with_ mutators, which produce new copies of the structs, is something that comes from the Scala/FP world of immutable objects. I’m not entirely sure how Rust-idiomatic the pattern is, but I like it and will continue to use it for Clone-able structs.

Moving on, we see the implementing struct:

```rust
    impl<P: PersistenceService + Send + Sync> OrderService for Orders<P> {
        fn create_order(&self, order: Order) -> Result<Order, String> {
            let url = format!(
                "https://{}/v1/accounts/{}/orders",
                self.base_url, self.account_id
            );
            let body = format!(
                "account_id={}&class=equity&symbol={}&side={}&quantity={}&type=market&duration=day",
                self.account_id, order.symbol, order.side, order.quantity
            );

            let response = post::<OrderResponse>(&url, &self.access_token, body);
            match response {
                Ok(response) => match response.order.status.as_str() {
                    "ok" => {
                        let new_order = order.with_id(response.order.id);
                        match self.persistence.write(Box::new(new_order.clone())) {
                            Ok(_) => {}
                            Err(e) => eprintln!("Error writing order: {}", e),
                        }

                        let position = position_from(&new_order, self.get_position(&order.symbol));
                        match self.persistence.write(Box::new(position.clone())) {
                            Ok(_) => self.update_position(&position),
                            Err(e) => eprintln!("Error writing position: {}", e),
                        }

                        Ok(new_order)
                    }
                    _ => Err(response.order.status),
                },
                Err(e) => Err(e),
            }
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

    fn position_from(order: &Order, existing: Option<Position>) -> Position {
        match order.side {
            Side::Buy => position_from_buy(order, existing),
            Side::Sell => position_from_sell(order, existing),
        }
    }

    fn position_from_buy(order: &Order, existing: Option<Position>) -> Position {
        match existing {
            Some(position) => Position {
                quantity: position.quantity + order.quantity,
                ..position
            },
            None => {
                Position {
                    // broker_id & cost_basis will be updated when positions are read from the broker
                    // These fields are not relevant to trading
                    broker_id: None,
                    symbol: order.symbol.clone(),
                    quantity: order.quantity,
                    cost_basis: order.px.unwrap_or(0.0) * order.quantity as f64, // Estimate
                    date: Local::now(),
                }
            }
        }
    }

    fn position_from_sell(order: &Order, existing: Option<Position>) -> Position {
        match existing {
            Some(position) => {
                assert!(
                    order.quantity == position.quantity,
                    "Attempted to sell more than owned"
                );
                Position {
                    quantity: 0,
                    ..position
                }
            }
            None => panic!("Attempted unwind with no position: {:?}", order),
        }
    }

```


As before, we’re using generics to specify service trait bounds, wrapping services in Arc for safe access, and using Mutex to interiorly-guard mutable data.

The pattern of using an implemtation-level Mutex keeps &mut self receivers out of interfaces. Those can be problematic in Rust — &mut self gives exclusive ownership, meaning that you cannot, for example, call such a method (or a combination of them, or even a single &mut self along with &self methods) more than once in the same scope.

Both the HTTP calls we’ve seen in this module use the core/http.rs utility discussed above.

Other things to note:

- A position is created/updated along with the order. That a market order for a relatively small quantity, for any reasonably liquid name, will be filled essentially instantaneously is a safe assumption. (The very complete Tradier API does offer an endpoint for monitoring active orders.)
- As discussed above, the persistence layer is asynchronous — those persistence.write calls (for the order and position) return immediately.

On to persistence.rs:

```rust
pub trait PersistenceService {
    fn init(&self, shutdown: Arc<AtomicBool>) -> Result<JoinHandle<()>, String>;
    fn write(&self, p: Box<dyn Persistable + Send>) -> Result<(), String>;
    fn drop_positions(&self) -> Result<(), String>;
}

pub fn new(url: String) -> Arc<impl PersistenceService> {
    let client = Client::with_uri_str(url).expect("Could not connect to MongoDB");
    let (sender, receiver) = crossbeam_channel::unbounded();
    Arc::new(implementation::Persistence {
        client,
        sender,
        receiver,
    })
}

mod implementation {
    use super::*;
    use mongodb::bson::{self, doc, Bson};
    use serde::Serialize;
    use std::any::Any;

    pub struct Persistence {
        pub client: Client,
        pub sender: Sender<Box<dyn Persistable + Send>>,
        pub receiver: Receiver<Box<dyn Persistable + Send>>,
    }

    pub struct Writer {
        pub client: Client,
        pub receiver: Receiver<Box<dyn Persistable + Send>>,
    }

    impl PersistenceService for Persistence {
        fn init(&self, shutdown: Arc<AtomicBool>) -> Result<JoinHandle<()>, String> {
            let client = self.client.clone();
            let receiver = self.receiver.clone();
            let writer = Writer { client, receiver };

            let handle = std::thread::spawn(move || {
                while !shutdown.load(std::sync::atomic::Ordering::Relaxed) {
                    match writer.receiver.recv() {
                        Ok(p) => match writer.write(p) {
                            Ok(_) => {}
                            Err(e) => {
                                eprintln!("Error writing object: {:?}", e);
                            }
                        },
                        Err(e) => {
                            eprintln!("Channel shut down: {:?}", e);
                        }
                    }
                }
            });
            Ok(handle)
        }

        fn write(&self, p: Box<dyn Persistable + Send>) -> Result<(), String> {
            self.sender.send(p).map_err(|e| e.to_string())
        }

        fn drop_positions(&self) -> Result<(), String> {
            self.client
                .database("algo-trading")
                .collection::<bson::Document>("positions")
                .drop(None)
                .map_err(|e| e.to_string())
        }
    }

    impl Writer {
        fn write(&self, p: Box<dyn Persistable>) -> Result<(), String> {
            if let Some(order) = p.as_any().downcast_ref::<Order>() {
                let filter: bson::Document = doc! {
                    "symbol": order.symbol.clone(),
                    "date": order.date.format("%Y-%m-%d").to_string()
                };
                self.upsert("orders", order.id(), filter, &order)
            } else if let Some(position) = p.as_any().downcast_ref::<Position>() {
                let filter: bson::Document = doc! { "symbol": position.symbol.clone() };
                self.upsert("positions", position.id(), filter, &position)
            } else {
                Err(format!("Cannot handle unknown type: {:?}", p.type_id()))
            }
        }

        fn upsert<T: ?Sized + Serialize>(
            &self,
            collection_name: &str,
            id: i64,
            filter: bson::Document,
            object: &T,
        ) -> Result<(), String> {
            let document: bson::Document = doc! {
                "$set": bson::to_bson(object).map_err(|e| e.to_string())?
            };
            let collection = self
                .client
                .database("algo-trading")
                .collection::<Bson>(collection_name);
            let options: mongodb::options::UpdateOptions =
                mongodb::options::UpdateOptions::builder()
                    .upsert(true)
                    .build();

            match collection.update_one(filter, document.to_owned(), options) {
                Ok(result) => {
                    let mongo_id = result
                        .upserted_id
                        .map(|id| id.as_object_id().expect("Cast to ObjectId failed"));
                    println!(
                        "Inserted/updated object with id, mongo id: {}, {:?}",
                        id, mongo_id
                    );
                }
                Err(e) => {
                    eprintln!("Error inserting object: {:?}; {:?}", e, id);
                }
            };
            Ok(())
        }
    }
}

```


You can see that we are using the same AtomicBool flag as a thread-shutdown mechanism as seen previously, and that the channel used to decouple callers from the database actions is hidden as an implementation detail.

Note also that the implementation is split into two structs. This makes sense, since the concerns of implementing the API methods are not the same as those of the writing, done asynchronously, but it’s worth noting that I didn’t have it that way originally — I was led in that direction by the borrow-checker. That experience made me realize that Rust’s obsession with memory safety can also lead to better modeling.

Another interesting aspect here is the mechanism used to abstract over Persistable objects:
```rust
pub trait Persistable {
    fn as_any(&self) -> &dyn Any;
    fn id(&self) -> i64;
}

...

impl Persistable for Order {
    fn as_any(&self) -> &dyn Any {
        self
    }

    fn id(&self) -> i64 {
        self.id.unwrap_or(0)
    }
}

...

impl Persistable for Position {
    fn as_any(&self) -> &dyn Any {
        self
    }

    fn id(&self) -> i64 {
        self.id.unwrap_or(0)
    }
}

```



The as_any cast to &dyn Any along with the use of downcast_ref is the way dynamic downcasting is done in Rust. I wanted to use a single channel for persistence, so this was the way to go.

This allowed me to write an upsert fn that is entirely agnostic to what is being written, as seen in the first gist above.

Finally, we’re to the good stuff — trading!

Trading strategies should be responsible for generating signals only — not placing orders.

```rust
impl StrategyHandler for Strategy {
    fn handle(&self, quote: &Quote, data: &SymbolData) -> Result<Signal, String> {
        match self {
            Strategy::MeanReversion { symbols } => {
                if symbols.contains(&quote.symbol) {
                    println!("MeanReversionStrategy handling quote: {:?}", quote);
                    println!(
                        "Px: {}; Mean: {}; Std Dev: {}",
                        quote.ask, data.mean, data.std_dev
                    );
                    println!(
                        "quote.ask: {}; (mean - 2.0 * std_dev): {}",
                        quote.ask,
                        data.mean - 2.0 * data.std_dev
                    );

                    let buy = quote.ask < data.mean - 2.0 * data.std_dev;
                    let sell = quote.ask > data.mean + 2.0 * data.std_dev;

                    if buy {
                        println!("***Buy signal for {}***", quote.symbol);
                        return Ok(Signal::Buy);
                    } else if sell {
                        println!("***Sell signal for {}***", quote.symbol);
                        return Ok(Signal::Sell);
                    } else {
                        println!("No signal for {}", quote.symbol);
                        return Ok(Signal::None);
                    }
                } else {
                    println!("Symbol {} not in strategy", quote.symbol);
                    return Ok(Signal::None);
                }
            }
        }
    }
}

```



The signals generated by this or any Strategy are handled by a TradingService instance:

```rust
    pub fn handle_quote(
        symbol_data: &HashMap<String, SymbolData>,
        quote: &Quote,
        capital: i64,
        strategy: &Strategy,
        orders: Arc<impl OrderService + 'static>,
    ) {
        if let Some(symbol_data) = symbol_data.get(&quote.symbol) {
            let maybe_position = orders.get_position(&quote.symbol);
            match strategy.handle(&quote, symbol_data) {
                Ok(signal) => match maybe_create_order(signal, maybe_position, quote, capital) {
                    Some(order) => match orders.create_order(order.clone()) {
                        Ok(o) => println!("Order created: {:?}", o),
                        Err(e) => eprintln!("Error creating order: {}", e),
                    },
                    None => (),
                },
                Err(e) => eprintln!("Error from strategy: {}", e),
            }
        } else {
            eprintln!("No symbol data found for {}", quote.symbol);
        }
    }

    pub fn maybe_create_order(
        signal: Signal,
        maybe_position: Option<Position>,
        quote: &Quote,
        capital: i64,
    ) -> Option<Order> {
        match signal {
            Signal::Buy => {
                // If position market value < capital, buy up to the limit
                let present_market_value = maybe_position
                    .map(|p| p.quantity as f64 * quote.ask)
                    .unwrap_or(0.0) as i64;
                let remaining_capital = capital - present_market_value;
                let shares = (remaining_capital as f64 / quote.ask) as i64;
                println!(
                    "Buy signal for {} at {}; present_market_value: {}; remaining_capital: {}; shares to buy: {}",
                    quote.symbol, quote.ask, present_market_value, remaining_capital, shares
                );

                match shares {
                    n if n > 0 => Some(Order {
                        symbol: quote.symbol.clone(),
                        quantity: shares,
                        date: Local::now().naive_local().date(),
                        side: Side::Buy,
                        broker_id: None,
                    }),
                    _ => {
                        println!("Buy signal for {}, but no capital", quote.symbol);
                        None
                    }
                }
            }

            Signal::Sell => {
                // If we have a position, unwind it all
                match maybe_position {
                    Some(p) => Some(Order {
                        symbol: quote.symbol.clone(),
                        quantity: p.quantity,
                        date: Local::now().naive_local().date(),
                        side: Side::Sell,
                        broker_id: None,
                    }),
                    None => {
                        println!(
                            "Sell signal for {}, but no position to unwind",
                            quote.symbol
                        );
                        None
                    }
                }
            }

            Signal::None => None,
        }
    }

```


The logic is as follows:

If the Strategy returns a Buy signal, we buy shares up to the capital allocated to the symbol minus the current position
If a Sell signal is generated, we unwind any existing position completely

Rust detail: I gave up on the ‘market_data lifetime. It became a matter of jumping through hoops, and I was already aware that ‘market_data was really the same as ‘static. Around this time I read somewhere that new Rust programmers have an unfounded aversion to using ‘static, and I realized that that was me. ‘static is not “bad” — it is what it is. If your data lives for the lifetime of the program, it’s ‘static — embrace it.

Also note that matching, even on primitives, is usually cleaner than an if-else.

Ok, let’s see a bit of the system in action:
![img.png](../assets/img/trading/img.pngts/img/trading/img.png)

A few interesting things happened here:

An AAPL Buy signal was generated. This was done by jury-rigging a fake quote of 100.0 (who wouldn’t load-up on Apple at that price?). The system calculated the shares to buy based on the name’s allocated capital ($100,000), the existing position’s market value ($0), and the (actual) ask of 212.89.
We see the logs from the asynchronous Mongo writes lower in the trace
A real Sell signal for AMZN was generated! Unfortunately, we had no position to unwind. If we did, we would have generated our first bit of alpha.
That’s it for now. The next installment will cover the critical area of backtesting, without which no trading algorithm should be allowed near real money.
