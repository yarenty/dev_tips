# Garnet

https://github.com/microsoft/garnet?tab=readme-ov-file


Garnet is a new remote cache-store from Microsoft Research, that offers several unique benefits:

- Garnet adopts the popular RESP wire protocol as a starting point, which makes it possible to use Garnet from unmodified Redis clients available in most programming languages of today, such as StackExchange.Redis in C#.
- Garnet offers much better throughput and scalability with many client connections and small batches, relative to comparable open-source cache-stores, leading to cost savings for large apps and services.
- Garnet demonstrates extremely low client latencies (often less than 300 microseconds at the 99.9th percentile) using commodity cloud (Azure) VMs with accelerated TCP enabled, which is critical to real-world scenarios.
- Based on the latest .NET technology, Garnet is cross-platform, extensible, and modern. It is designed to be easy to develop for and evolve, without sacrificing performance in the common case. We leveraged the rich library ecosystem of .NET for API breadth, with open opportunities for optimization. Thanks to our careful use of .NET, Garnet achieves state-of-the-art performance on both Linux and Windows.

This repo contains the code to build and run Garnet. For more information and documentation, check out our website at https://microsoft.github.io/garnet.

