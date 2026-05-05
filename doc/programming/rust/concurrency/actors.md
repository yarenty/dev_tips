---
title: Actix
main_link: https://crates.io/crates/actix
keywords: [actors, rust, messages, self]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Actix

**Main link:** <https://crates.io/crates/actix>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[ised]] — Iced _(score 28.4)_
- [[streaming]] — par-stream _(score 17.1)_
- [[timely]] — Timely Dataflow _(score 17.1)_
- [[cargo_toml]] — Cargo.toml _(score 13.1)_
- [[rtic]] — RTIC _(score 13.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#actors` `#concurrency` `#rust` `#programming` `#actor` `#message` `#system` `#messages`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Actix

https://crates.io/crates/actix


## Features
- Async and sync actors
- Actor communication in a local/thread context
- Uses futures for asynchronous message handling
- Actor supervision
- Typed messages (No Any type)
- Runs on stable Rust 1.54+

## Usage
To use actix, add this to your Cargo.toml:

```toml
[dependencies]
actix = "0.12"
```

## Initialize Actix
In order to use actix you first need to create a System.
```rust
fn main() {
let system = actix::System::new();

    system.run();
}
```
Actix uses the Tokio runtime. System::new() creates a new event loop. System.run() starts the Tokio event loop, and will finish once the System actor receives the SystemExit message.

## Implementing an Actor
In order to define an actor you need to define a struct and have it implement the Actor trait.
```rust
use actix::{Actor, Addr, Context, System};

struct MyActor;

impl Actor for MyActor {
type Context = Context<Self>;

    fn started(&mut self, ctx: &mut Self::Context) {
        println!("I am alive!");
        System::current().stop(); // <- stop system
    }
}

fn main() {
let mut system = System::new();

    let addr = system.block_on(async { MyActor.start() });

    system.run();
}
```
Spawning a new actor is achieved via the start and create methods of the Actor trait. It provides several different ways of creating actors; for details, check the docs. You can implement the started, stopping and stopped methods of the Actor trait. started gets called when the actor starts and stopping when the actor finishes. Check the API docs for more information on the actor lifecycle.

## Handle Messages
An Actor communicates with another Actor by sending messages. In actix all messages are typed. Let's define a simple Sum message with two usize parameters and an actor which will accept this message and return the sum of those two numbers. Here we use the #[actix::main] attribute as an easier way to start our System and drive our main function so we can easily .await for the responses sent back from the Actor.
```rust
use actix::prelude::*;

// this is our Message
// we have to define the response type (rtype)
#[derive(Message)]
#[rtype(result = "usize")]
struct Sum(usize, usize);

// Actor definition
struct Calculator;

impl Actor for Calculator {
type Context = Context<Self>;
}

// now we need to implement `Handler` on `Calculator` for the `Sum` message.
impl Handler<Sum> for Calculator {
type Result = usize; // <- Message response type

    fn handle(&mut self, msg: Sum, ctx: &mut Context<Self>) -> Self::Result {
        msg.0 + msg.1
    }
}

#[actix::main] // <- starts the system and block until future resolves
async fn main() {
let addr = Calculator.start();
let res = addr.send(Sum(10, 5)).await; // <- send message and get future for result

    match res {
        Ok(result) => println!("SUM: {}", result),
        _ => println!("Communication to the actor has failed"),
    }
}
```
All communications with actors go through an Addr object. You can do_send a message without waiting for a response, or you can send an actor a specific message. The Message trait defines the result type for a message.


## Actor State And Subscription For Specific Messages
You may have noticed that the methods of the Actor and Handler traits accept &mut self, so you are welcome to store anything in an actor and mutate it whenever necessary.

Address objects require an actor type, but if we just want to send a specific message to an actor that can handle the message, we can use the Recipient interface. Let's create a new actor that uses Recipient.
```rust
use actix::prelude::*;
use std::time::Duration;

#[derive(Message)]
#[rtype(result = "()")]
struct Ping {
pub id: usize,
}

// Actor definition
struct Game {
counter: usize,
name: String,
recipient: Recipient<Ping>,
}

impl Actor for Game {
type Context = Context<Game>;
}

// simple message handler for Ping message
impl Handler<Ping> for Game {
type Result = ();

    fn handle(&mut self, msg: Ping, ctx: &mut Context<Self>) {
        self.counter += 1;

        if self.counter > 10 {
            System::current().stop();
        } else {
            println!("[{0}] Ping received {1}", self.name, msg.id);

            // wait 100 nanoseconds
            ctx.run_later(Duration::new(0, 100), move |act, _| {
                act.recipient.do_send(Ping { id: msg.id + 1 });
            });
        }
    }
}

fn main() {
let mut system = System::new();

    // To get a Recipient object, we need to use a different builder method
    // which will allow postponing actor creation
    let addr = system.block_on(async {
        Game::create(|ctx| {
            // now we can get an address of the first actor and create the second actor
            let addr = ctx.address();

            let addr2 = Game {
                counter: 0,
                name: String::from("Game 2"),
                recipient: addr.recipient(),
            }
            .start();

            // let's start pings
            addr2.do_send(Ping { id: 10 });

            // now we can finally create first actor
            Game {
                counter: 0,
                name: String::from("Game 1"),
                recipient: addr2.recipient(),
            }
        });
    });

    system.run();
}
```
