# Tracing

https://github.com/tokio-rs/tracing

tracing is a framework for instrumenting Rust programs to collect structured, event-based diagnostic information. tracing is maintained by the Tokio project, but does not require the tokio runtime to be used.




# rust-prometheus form tikv

https://github.com/tikv/rust-prometheus


Find the latest documentation at https://docs.rs/prometheus.

## Crate features
This crate provides several optional components which can be enabled via Cargo [features]:

- gen: To generate protobuf client with the latest protobuf version instead of using the pre-generated client.

- nightly: Enable nightly only features.

- process: Enable process metrics support.

- push: Enable push metrics support.

Static Metric
When using a MetricVec with label values known at compile time prometheus-static-metric reduces the overhead of retrieving the concrete Metric from a MetricVec.

See static-metric directory for details.


# Loki


inspired by prometeus

quite interesting

