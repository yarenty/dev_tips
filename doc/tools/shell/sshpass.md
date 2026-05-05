---
title: sshpass
main_link: 
keywords: [sshpass, shell, tools, replacement, test]
status: draft
---

<!-- auto-stubbed by article_stub.py -->

# sshpass

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[dns_toys]] — Dns Toys _(score 25.1)_
- [[tools/shell/tools|tools]] — Shell tools _(score 25.1)_
- [[lynx]] — Lynx _(score 25.1)_
- [[tools/shell/tmux|tmux]] — Tmux _(score 25.1)_
- [[must_have]] — Commands to install _(score 25.1)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#sshpass` `#shell` `#tools` `#replacement` `#example` `#create` `#test`

## TODO

- No `main_link` could be auto-detected. Add the canonical URL (project homepage / repo / paper) to the frontmatter.
- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# sshpass

```shell
sshpass -p "password" scp -r user@example.com:/some/remote/path /some/local/path
```

```shell
sshpass -f "/path/to/passwordfile" scp -r user@example.com:/some/remote/path /some/local/path
```


replacement of sshpass:

For example create 'test.exp' :

```shell
#!/usr/bin/expect
        spawn scp  /usr/bin/file.txt root@<ServerLocation>:/home
        set pass "Your_Password"
        expect {
        password: {send "$pass\r"; exp_continue}
                  }
```
run the script

```shell
expect test.exp 
```
