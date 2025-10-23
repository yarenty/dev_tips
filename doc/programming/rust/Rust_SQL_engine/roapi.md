# ROAPI

ROAPI automatically spins up read-only APIs for static datasets without requiring you to write a single line of code. It builds on top of Apache Arrow and Datafusion. The core of its design can be boiled down to the following:

Query frontends to translate SQL, GraphQL and REST API queries into Datafusion plans.
Datafusion for query plan execution.
Data layer to load datasets from a variety of sources and formats with automatic schema inference.
Response encoding layer to serialize intermediate Arrow record batch into various formats requested by client.


![](https://camo.githubusercontent.com/6846b7b40ddfc780fc97c2238fd7a4ea2594bde8883adf30fb0dc8086185c80d/68747470733a2f2f726f6170692e6769746875622e696f2f646f63732f696d616765732f726f6170692e706e67)

https://github.com/roapi/roapi/blob/main/columnq/src/query/graphql.rs
