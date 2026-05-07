---
title: "dns.toys: a DNS server full of useful tricks"
main_link: https://www.dns.toys/
keywords: [dns-toys, dns, dig, kailash-nadh, weather, currency, conversions, shell]
status: reviewed
---

# dns.toys: a DNS server full of useful tricks

**Main link:** <https://www.dns.toys/>

Repo: <https://github.com/knadh/dns.toys>

## Summary

`dns.toys` is a public DNS server by Kailash Nadh that answers nonsense
queries with useful data: world time, weather, currency conversion, unit
conversion, base conversion, CIDR maths, your public IP, even digits of π.
You query it with `dig`, which means it works from any box on the planet
without curl, without an HTTPS client, without an API key, and through
firewalls that block almost everything except UDP/53 and TCP/53.

## Insight

Treat it as a "fun toolbelt over UDP/53" — the use case is one-off
conversions and quick lookups in a shell where you don't want to spin up
Python or hit a rate-limited HTTP API. Wrap it in a `dy` shell function
(see below) and `dy 100USD-EUR.fx` becomes faster than opening a converter
website.

Don't use it as production infrastructure: it's a free, best-effort service
run by one person, and any DNS-based exfil channel is by definition shady.
It is, however, a beautiful demo of how flexible the DNS protocol is when
you stop pretending it's only for hostnames.

## Similar / related topics

- [`wttr.in`](https://wttr.in/) — same idea over HTTP: `curl wttr.in/london`.
- [`cheat.sh`](https://cheat.sh/) — community cheat sheets via `curl`.
- [`qrenco.de`](https://qrenco.de/) — `curl qrenco.de/string` returns a QR code in your terminal.
- [[fish]] — pair this with a shell that has good aliases and abbreviations.
- [[tools/shell/tools|shell tools]] — sister page of "shell ergonomics" tools.

## Internal links

- [[fish]]
- [[must_have]]
- [[tools/shell/tools|shell tools]]
- [[color_codes]]

## Keywords

`#dns-toys` `#dns` `#dig` `#weather` `#currency` `#conversions` `#shell` `#tricks`

## References / raw notes

All examples assume `dig` is installed (`brew install bind` /
`apt install dnsutils`).

### World time

```shell
dig mumbai.time @dns.toys
dig newyork.time @dns.toys
dig paris/fr.time @dns.toys
```

City names go without spaces, suffixed with `.time`. Two-letter country
codes are optional disambiguators.

### Weather (powered by yr.no)

```shell
dig mumbai.weather @dns.toys
dig newyork.weather @dns.toys
dig amsterdam/nl.weather @dns.toys
```

### Unit conversion

```shell
dig 42km-mi.unit @dns.toys
dig 32GB-MB.unit @dns.toys
```

Format: `$Value$FromUnit-$ToUnit`. Run `dig unit @dns.toys` for the full
list of ~70 supported units.

### Currency / forex

```shell
dig 100USD-INR.fx @dns.toys
dig 50CAD-AUD.fx @dns.toys
```

Daily rates from `exchangerate.host`.

### IP echo

```shell
dig ip @dns.toys
```

### Number to words

```shell
dig 987654321.words @dns.toys
```

### Usable CIDR range

```shell
dig 10.0.0.0/24.cidr @dns.toys
dig 2001:db8::/108.cidr @dns.toys
```

### Number base conversion

```shell
dig 100dec-hex.base @dns.toys
dig 755oct-bin.base @dns.toys
```

Bases: `hex`, `dec`, `oct`, `bin`.

### Pi

```shell
dig pi @dns.toys
dig pi -t txt @dns.toys
dig pi -t aaaa @dns.toys
```

### Help

```shell
dig help @dns.toys
```

### Shortcut function

`bash` (drop in `~/.bashrc`):

```shell
function dy { dig +noall +answer +additional "$1" @dns.toys; }
```

`fish` (drop in `~/.config/fish/config.fish`):

```fish
function dy
    dig +noall +answer +additional $argv[1] @dns.toys
end
```

Then:

```shell
dy berlin.time
dy mumbai.weather
dy 100USD-INR.fx
```
