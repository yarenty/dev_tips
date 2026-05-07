---
title: armada — high-performance TCP SYN scanner
main_link: https://github.com/resyncgg/armada
keywords: [armada, rust, tcp, syn-scan, port-scan, security, network]
status: reviewed
---

# armada — high-performance TCP SYN scanner

**Main link:** <https://github.com/resyncgg/armada>

## Summary

`armada` is a high-performance TCP SYN scanner by Resync (resyncgg) — equivalent to `nmap -sS`, focused tightly on answering "is this port open?" at scale. It does not do service / OS fingerprinting; it just scans (very) fast and gets out of the way, leaving deeper inspection to whatever tooling you point at the open ports it found. Filed here as a built-with-Rust CLI; the broader port-scanner article in the security tools section covers RustScan (a different tool with similar goals).

## Insight

Reach for `armada` when you need to scan a large IP range for open ports and `nmap -sS` is too slow. The trade-off is honest: armada is *only* a scanner — no NSE scripts, no version-detection probes, no OS guessing. The expected workflow is **armada to find open ports, then nmap / your-favourite-tool to enumerate**.

**Compared to siblings**:

- **`nmap`** — the universal default; slower at scale but does everything (service detection, scripts, OS fingerprint).
- **[[../../../tools/security/rustscan|rustscan]]** — the Rust scanner most people reach for first; pipes results into nmap by default. Generally a better starting point than armada unless you have very specific scale needs.
- **`masscan`** (C) — the long-standing fast-scanner predecessor; can scan the entire IPv4 space in minutes given enough bandwidth.
- **`zmap`** — even faster, single-port internet-wide scanner; the academic / research tool.

**Ethics note**: SYN scanning *is* port scanning, and unsolicited port scans against networks you don't own can violate computer-misuse laws in most jurisdictions, regardless of whether you "do anything" with the results. Limit use to networks you own, networks where you have explicit written authorisation, or recognised public-research scopes.

## Similar / related topics

- `nmap` — the universal port scanner.
- [[../../../tools/security/rustscan|rustscan]] — Rust scanner, pipes into nmap; the more common starting point.
- `masscan` (C) — fast internet-scale scanner.
- `zmap` — single-port internet-scale scanner.

## Internal links

<!-- reviewed -->

- [[README]] — tooling section landing.
- [[../../../tools/security/rustscan|rustscan]] — sibling Rust scanner article.
- [[../../../tools/security/README|tools/security/]] — broader defensive / offensive tools landing.

## Keywords

`#armada` `#rust` `#tcp` `#syn-scan` `#port-scan` `#network` `#security` `#cli`

## References / raw notes

- Repo: <https://github.com/resyncgg/armada>
- TCP SYN scanner; equivalent to `nmap -sS`.
- Goal: answer "is this port open?" at scale; leave service identification to other tooling.
