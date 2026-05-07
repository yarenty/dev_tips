---
title: "Shodan: search engine for internet-exposed devices"
main_link: https://www.shodan.io/
keywords: [shodan, iot, security, recon, internet-of-things, port-scanning, attack-surface]
status: reviewed
---

# Shodan: search engine for internet-exposed devices

**Main link:** <https://www.shodan.io/>

CLI / library: <https://github.com/achillean/shodan-python> · API docs: <https://developer.shodan.io/>

## Summary

[Shodan](https://www.shodan.io/) is a continuous internet-wide port scanner that indexes everything it finds — open ports, service banners, TLS certificates, HTTP titles, default credentials, ICS/SCADA fingerprints, exposed databases, IP cameras, MQTT brokers, RDP endpoints, MongoDB instances without auth — and exposes it via a search engine and REST API. Founded by [John Matherly](https://twitter.com/achillean) in 2009, it's the canonical tool for understanding what your *own* IPs look like from the outside, and (less defensibly) what *other people's* exposed services look like.

## Insight

Shodan is one of those tools whose use is shaped entirely by your intent:

- **Defensive recon (the legitimate use)**: paste your company's IP ranges into Shodan to see what your perimeter actually exposes — forgotten dev VMs, mis-configured S3 buckets that fronted with a public endpoint, accidentally-exposed Kubernetes API servers, IoT devices that escaped the network team's inventory. Almost every company has Shodan-visible things they didn't know about; an hour with Shodan + your CIDR list is one of the highest-leverage security exercises you'll ever do.
- **Threat-intel research**: track how many MongoDB instances are still exposed without auth (the answer is "still depressingly many"), or look at the spread of a specific CVE in the wild.
- **Looking at other people's stuff**: legally a grey area in most jurisdictions even though Shodan only re-indexes what's publicly responding. Don't.

The free tier is enough to verify your own footprint manually. The paid tiers ([Membership](https://account.shodan.io/billing) is a one-time ~$49 lifetime fee, often discounted to $5 on Black Friday) unlock filters (`org:`, `net:`, `ssl.cert.subject.cn:`, `port:`, `country:`, `vuln:`) which is where the tool actually becomes powerful.

The IoT angle (why this article lives in `doc/iot/`): Shodan's specialty is the long tail of *embedded* devices — Hikvision cameras, Niagara industrial controllers, exposed printers, MQTT brokers, default-password routers. If you're building IoT (see [[drogue]], [[balena]]), running Shodan against your fleet's IP block tells you fast whether your devices are leaking telemetry, exposing config interfaces, or running with default creds.

## Similar / related topics

- [Censys](https://search.censys.io/) — direct competitor; broader cert / DNS focus, often-cleaner API.
- [BinaryEdge](https://www.binaryedge.io/) — another internet-scan-as-a-service shop.
- [ZoomEye](https://www.zoomeye.org/) — Chinese equivalent with an Asia-Pacific bias.
- [GreyNoise](https://www.greynoise.io/) — flips the question: instead of "what's exposed?", "who's scanning the internet right now?". Highly complementary.
- [`nmap`](https://nmap.org/) — for scanning *your own* hosts in detail; Shodan is the wide-and-shallow internet view, nmap is the deep-and-narrow point-scan.

## Internal links

- [[drogue]] — IoT framework that benefits from a Shodan check on the deployed fleet
- [[messaging/mqtt|mqtt]] — *the* IoT protocol; lots of exposed brokers visible to Shodan

## Keywords

`#iot` `#shodan` `#security` `#recon` `#internet-of-things` `#port-scanning` `#attack-surface`

## References / raw notes

### Pitch (from the site)

> Search Engine for the Internet of Everything. Shodan is the world's first search engine for Internet-connected devices. Discover how Internet intelligence can help you make better decisions.

### Useful entry points

- Web search: <https://www.shodan.io/>
- Filter reference (paid tier): <https://www.shodan.io/search/filters>
- Public dashboards (free): <https://www.shodan.io/explore>
- API docs: <https://developer.shodan.io/>
- Python CLI / library: <https://github.com/achillean/shodan-python>
- *Shodan, the Internet's most dangerous search engine* (Wired, 2013, gives the historical context): <https://www.wired.com/2013/04/shodan-search-engine/>

### Quick CLI workflow

```sh
pip install shodan
shodan init <YOUR_API_KEY>     # one-time

# What's open on a single IP?
shodan host 8.8.8.8

# What does my CIDR look like?
shodan search 'net:203.0.113.0/24'

# Exposed MQTT brokers running Mosquitto
shodan search 'product:Mosquitto'

# MongoDB instances with no auth, in DE
shodan search 'product:MongoDB country:DE'

# Hikvision cameras
shodan search 'Server: Hikvision-Webs'
```

### What to look for when auditing your own perimeter

- HTTP titles that reveal admin panels: `port:80,443 org:"YourCompany" "Welcome"`
- Default cert CNs from cloud providers leaking into prod: `ssl.cert.subject.cn:"*.elb.amazonaws.com"`
- RDP / VNC / SSH on non-standard ports: `port:3389 net:<your CIDR>`
- Database ports: `port:5432,3306,27017,6379 net:<your CIDR>`
- Kubernetes API servers without auth: `port:6443 "kubernetes" net:<your CIDR>`
