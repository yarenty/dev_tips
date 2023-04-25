
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