# Trade Aggregation

https://github.com/MathisWellmann/trade_aggregation-rs?tab=readme-ov-file

A high performance, modular and flexible trade aggregation crate, producing Candle data, suitable for low-latency applications and incremental updates. It allows the user to choose the rule dictating how a new candle is created through the AggregationRule trait, e.g: Time, Volume based or some other information driven rule. It also allows the user to choose which type of candle will be created from the aggregation process through the ModularCandle trait. Combined with the Candle macro, it enables the user to flexibly create any type of Candle as long as each component implements the CandleComponent trait. The aggregation process is also generic over the type of input trade data as long as it implements the TakerTrade trait, allowing for greater flexibility for downstream projects.

See MathisWellmann/go_trade_aggregation for a go implementation with less features and performance.