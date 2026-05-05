---
title: Wireguard
main_link: https://www.wireguard.com/talks/talk-demo-screencast.mp4
keywords: [wireguard, security, tools, nat, wg0, peer, key]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# Wireguard

**Main link:** <https://www.wireguard.com/talks/talk-demo-screencast.mp4>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#wireguard` `#security` `#tools` `#nat` `#wg0` `#peer` `#key`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes
<!-- auto-split by article_split.py -->
> Auto-split: 7 additional top-level heading(s) extracted into sibling files:
> - [ip link add dev wg0 type wireguard](ip_link_add_dev_wg0_type_wireguard.md)
> - [ip address add dev wg0 192.168.2.1/24](ip_address_add_dev_wg0_192_168_2_1_24.md)
> - [ip address add dev wg0 192.168.2.1 peer 192.168.2.2](ip_address_add_dev_wg0_192_168_2_1_peer_192_168_2_2.md)
> - [wg setconf wg0 myconfig.conf](wg_setconf_wg0_myconfig_conf.md)
> - [wg set wg0 listen-port 51820 private-key /path/to/private-key peer ABCDEF... allowed-ips 192.168.88.0/24 endpoint 209.202.254.14:8172](wg_set_wg0_listen_port_51820_private_key_path_to_private_key.md)
> - [ip link set up dev wg0](ip_link_set_up_dev_wg0.md)
> - [modprobe wireguard && echo module wireguard +p > /sys/kernel/debug/dynamic_debug/control](modprobe_wireguard_echo_module_wireguard_p_sys_kernel_debug_.md)


<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Wireguard


## Quick Start
You'll first want to make sure you have a decent grasp of the conceptual overview, and then install WireGuard. After that, read onwards here.

## Side by Side Video
Before explaining the actual comands in detail, it may be extremely instructive to first watch them being used by two peers being configured side by side:


[https://www.wireguard.com/talks/talk-demo-screencast.mp4](https://www.wireguard.com/talks/talk-demo-screencast.mp4)



Or individually, a single configuration looks like:


![](https://www.wireguard.com/img/walkthrough.gif)



Command-line Interface
A new interface can be added via ip-link(8), which should automatically handle module loading:
