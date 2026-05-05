---
title: coreutils
main_link: https://github.com/Zij-IT/clido
keywords: [tools, tooling, rust, programming, crates, linux, ripgrep, windows]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# coreutils

**Main link:** <https://github.com/Zij-IT/clido>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- TODO: at least 2 [[wikilinks]] to related articles in this vault. -->

## Keywords

`#tools` `#tooling` `#rust` `#programming` `#crates` `#linux` `#ripgrep` `#windows`

## TODO

- This file contains **19 top-level `#` headings** — it likely covers multiple distinct topics. Per plan.md §8 step 3, **split this file** into one article per topic.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# coreutils

cargo isntall coreutile


# dusk  - replacement of du

cargo install du-dusk

dusk



# mprocs - replacement of tmux for long running processes! 
like DB, logs , ...

cargo install mprocs


# zellij - replacement of tmux - with colors and stuff

cargo install zellij


# ripgrep

https://crates.io/crates/ripgrep


ripgrep is a line-oriented search tool that recursively searches the current directory for a regex pattern. By default, ripgrep will respect gitignore rules and automatically skip hidden files/directories and binary files. ripgrep has first class support on Windows, macOS and Linux, with binary downloads available for every release. ripgrep is similar to other popular search tools like The Silver Searcher, ack and grep.

cargo install ripgrep

rg test



# BAT

Like cat but with syntax highlights of displaying unvisibles

https://crates.io/crates/bat

A cat(1) clone with wings.


# EXA

new ls


https://crates.io/crates/exa

exa is a modern replacement for the venerable file-listing command-line program ls that ships with Unix and Linux operating systems, giving it more features and better defaults. It uses colours to distinguish file types and metadata. It knows about symlinks, extended attributes, and Git. And it’s small, fast, and just one single binary.

By deliberately making some decisions differently, exa attempts to be a more featureful, more user-friendly version of ls. For more information, see exa’s website.


# bottom

NICE TEXT GRAPHS

https://crates.io/crates/bottom


A customizable cross-platform graphical process/system monitor for the terminal. Supports Linux, macOS, and Windows.
#tui #cross-platform #cli #monitoring #top



# RUST in JUPYTER

cargo install evcxr_jupyter

and just: jupyter notebook


----




# zoxide

https://crates.io/crates/zoxide

zoxide is a smarter cd command, inspired by z and autojump.

It remembers which directories you use most frequently, so you can "jump" to them in just a few keystrokes.
zoxide works on all major shells.



# clido

https://github.com/Zij-IT/clido

TODO list in CLI



# joshuto

https://crates.io/crates/joshuto

Terminal file manager inspired by ranger


https://github.com/kamiyaa/joshuto


# topgrade

https://crates.io/crates/topgrade

https://github.com/r-darwish/topgrade

Keeping your system up to date usually involves invoking multiple package managers. This results in big, non-portable shell one-liners saved in your shell. To remedy this, topgrade detects which tools you use and runs the appropriate commands to update them.




# Bandwhich

https://crates.io/crates/bandwhich

This is a CLI utility for displaying current network utilization by process, connection and remote IP/hostname

How does it work?
bandwhich sniffs a given network interface and records IP packet size, cross referencing it with the /proc filesystem on linux, lsof on macOS, or using WinApi on windows. It is responsive to the terminal window size, displaying less info if there is no room for it. It will also attempt to resolve ips to their host name in the background using reverse DNS on a best effort basis.





# starship

https://crates.io/crates/starship

The minimal, blazing-fast, and infinitely customizable prompt for any shell!

Fast: it's fast – really really fast! 🚀
Customizable: configure every aspect of your prompt.
Universal: works on any shell, on any operating system.
Intelligent: shows relevant information at a glance.
Feature rich: support for all your favorite tools.
Easy: quick to install – start using it in minutes.




# difftastic

https://crates.io/crates/difftastic

Difftastic is a structural diff tool that compares files based on their syntax.


# armada

TCP scanner

https://github.com/resyncgg/armada

Armada is a high performance TCP SYN scanner. This is equivalent to the type of scanning that nmap might perform when you use the -sS scan type. Armada's main goal is to answer the basic question "Is this port open?". It is then up to you, or your tooling, to dig further to identify what an open port is for.





# watchexec

https://crates.io/crates/watchexec

https://github.com/watchexec/watchexec



Software development often involves running the same commands over and over. Boring!

watchexec is a simple, standalone tool that watches a path and runs a command whenever it detects modifications.

Example use cases:

Automatically run unit tests
Run linters/syntax checkers
Features
Simple invocation and use, does not require a cryptic command line involving xargs
Runs on OS X, Linux, and Windows
Monitors current directory and all subdirectories for changes
Coalesces multiple filesystem events into one, for editors that use swap/backup files during saving
Loads .gitignore and .ignore files
Uses process groups to keep hold of forking programs
Provides the paths that changed in environment variables
Does not require a language runtime, not tied to any particular language or ecosystem
And more!




# lychee

Link checker
 
https://github.com/lycheeverse/lychee/tree/master



⚡ A fast, async, stream-based link checker written in Rust.
Finds broken hyperlinks and mail addresses inside Markdown, HTML, reStructuredText, or any other text file or website!

Available as a command-line utility, a library and a GitHub Action.
