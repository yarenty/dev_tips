---
title: "RustScan — fast Rust port scanner"
main_link: https://github.com/bee-san/RustScan
keywords: [rustscan, port-scanner, nmap, rust, recon, security, network-discovery]
status: reviewed
---

# RustScan — fast Rust port scanner

**Main link:** <https://github.com/bee-san/RustScan>

## Summary

RustScan is a port scanner written in Rust that finds all open ports on a host very quickly (the marketing line is "all 65k ports in 3 seconds") and then pipes the results straight into nmap for the slow service-detection / scripting pass. It is not a replacement for nmap — it's a fast front-end that hands nmap a much shorter port list to chew on.

## Insight

The right mental model: **nmap's discovery pass is slow because it scans 1000 default ports and then maybe `-p-` for everything**. RustScan parallelises that discovery aggressively, so you get the open-port list in seconds, then `nmap -sV -sC` on just those ports finishes in seconds more instead of minutes. The pipeline is the whole point.

When to reach for it:

- CTFs and recon — the "fast nmap" sells itself.
- Inventorying an unfamiliar internal subnet.
- Anywhere you'd otherwise type `nmap -p- -T4 host` and walk away to make coffee.

Gotchas:

- **You can melt your NIC, your router, or someone's IDS.** RustScan defaults are aggressive. Use `--ulimit` and `-b` (batch size) to tone it down on weak networks or when scanning over VPN.
- **It's still nmap underneath** for the interesting work. If nmap isn't installed (or RustScan can't find it), you only get the open-port list.
- **Don't scan things you don't own.** Unauthorised port scanning is a crime in many jurisdictions and unambiguously rude everywhere else. For internet-wide reconnaissance, look at [[shodan]] or [Censys](https://censys.io/) results instead of running it yourself.
- The repo moved from `RustScan/RustScan` to `bee-san/RustScan`; old links still redirect.

## Similar / related topics

- [nmap](https://nmap.org/) — the classic port scanner / service fingerprinter RustScan wraps.
- [masscan](https://github.com/robertdavidgraham/masscan) — even faster, asynchronous, designed for internet-scale scans.
- [naabu](https://github.com/projectdiscovery/naabu) — Go port scanner from ProjectDiscovery; similar niche.
- [[shodan]] — pre-scanned internet, so you don't have to scan it yourself.
- [[sniffnet]] — passive counterpart: watch what's already on the wire instead of probing.

## Internal links

- [[sniffnet]]
- [[shodan]]
- [[mtr]]
- [[wireguard]]

## Keywords

`#rustscan` `#port-scanner` `#nmap` `#rust` `#recon` `#security` `#network-discovery` `#tools`

## References / raw notes

> Fast, smart, effective.

![RustScan demo](https://github.com/bee-san/RustScan/raw/master/pictures/fast.gif)

Typical invocation:

```shell
rustscan -a 10.0.0.0/24                 # scan a subnet
rustscan -a target.example -- -sV -sC   # everything after `--` is forwarded to nmap
rustscan -a target.example -p 1-1024 --ulimit 5000
```
