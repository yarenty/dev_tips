---
title: Tmux AI
main_link: https://tmuxai.dev/
keywords: [tmux-ai, linux, tools, tmux, dev]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

> Auto-split from `doc/tools/linux/tmux.md` by `article_split.py`. Heading: **Tmux AI**.

# Tmux AI

**Main link:** <https://tmuxai.dev/>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[tools/linux/tmux|tmux]] — TMUX _(score 28)_
- [[tools/shell/tmux|tmux]] — Tmux _(score 20)_
- [[tools/linux/zellij|zellij]] — Zellij _(score 18)_
- [[fish]] — Fish _(score 13)_
- [[helix]] — Helix _(score 13)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->

## Keywords

`#tmux-ai` `#linux` `#tools` `#tmux` `#tmuxai` `#dev`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Tmux AI


https://tmuxai.dev/

```shell
CHAT PANE
TmuxAI» start docker container mysql and then connect to mysql shell.
First I'll start the container:
TmuxAI» docker run --name mysql_container -e MYSQL_ROOT_PASSWORD=password -d mysql:latest
Do you want to execute this command? [Y]es/No/Edit:
TmuxAI» Now I will connect to the mysql shell.
TmuxAI» docker exec -it mysql_container mysql -u root -p
Do you want to execute this command? [Y]es/No/Edit:
TmuxAI» I'll now send the password to the mysql shell.
TmuxAI» Sending keys: password
TmuxAI» Sending keys: Enter
TmuxAI» I've successfully connected to the MySQL shell.

```
