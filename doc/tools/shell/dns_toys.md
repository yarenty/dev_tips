---
title: Dns Toys
main_link: https://www.dns.toys/
keywords: [dns-toys, shell, tools, pass, number, value, cidr]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Dns Toys

**Main link:** <https://www.dns.toys/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#dns-toys` `#shell` `#tools` `#pass` `#number` `#value` `#cidr`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

https://www.dns.toys/

Copy and run the below commands to try it out.

World time
```shell

dig mumbai.time @dns.toys

dig newyork.time @dns.toys

dig paris/fr.time @dns.toys
```

Pass city names without spaces suffixed with .time. Pass two letter country codes optionally.

Weather
```shell
dig mumbai.weather @dns.toys

dig newyork.weather @dns.toys

dig amsterdam/nl.weather @dns.toys
```
Pass city names without spaces suffixed with .weather. Pass two letter country codes optionally. This service is powered by yr.no

Unit conversion
```shell
dig 42km-mi.unit @dns.toys

dig 32GB-MB.unit @dns.toys
```
$Value$FromUnit-$ToUnit. To see all 70 available units, dig unit @dns.toys

Currency conversion (forex)
```shell
dig 100USD-INR.fx @dns.toys

dig 50CAD-AUD.fx @dns.toys
```
$Value$FromCurrency-$ToCurrency. Daily rates are from exchangerate.host.

IP echo
```shell
dig ip @dns.toys
```
Echo your IP address.

Number to words
```shell
dig 987654321.words @dns.toys
```
Convert numbers to English words.

Usable CIDR Range
```shell
dig 10.0.0.0/24.cidr @dns.toys

dig 2001:db8::/108.cidr @dns.toys
```

Parse CIDR notation to find out first and last usable IP address in the subnet.

Number base conversion
```shell
dig 100dec-hex.base @dns.toys

dig 755oct-bin.base @dns.toys
```

Converts a number from one base to another. Supported bases are hex, dec, oct and bin.

Pi
```shell
dig pi @dns.toys

dig pi -t txt @dns.toys

dig pi -t aaaa @dns.toys
```
Print digits of Pi. Yep.

Help
```shell
dig help @dns.toys
```
Lists available services.

Shortcut function
Bash
Add this bash function to your ~/.bashrc file. The + args show cleaner output from dig.
```shell
function dy { dig +noall +answer +additional "$1" @dns.toys; }
```
Fish
Add this to your fish config file.
```shell
alias dy="dig +noall +answer +additional $argv[1] @dns.toys"
```
Then, use the dy command as a shortcut.
```shell
dy berlin.time

dy mumbai.weather

dy 100USD-INR.fx
```
