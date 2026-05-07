---
title: Git configuration — sane defaults
main_link: https://blog.gitbutler.com/how-git-core-devs-configure-git/
keywords: [git, config, gitconfig, defaults, diff, rebase, push, fetch]
status: reviewed
---

# Git configuration — sane defaults

**Main link:** <https://blog.gitbutler.com/how-git-core-devs-configure-git/>

## Summary

A copy-paste-able `~/.gitconfig` snippet that turns Git into the tool it should already have been by default. Sourced from a GitButler blog post that distilled the "Spring Cleaning" thread on the Git mailing list, where Git's own core developers compared the settings they cannot live without. Most are safe global toggles (better diff algorithm, smarter push/fetch, sorted branch listings); a few are matters of taste (rebase-on-pull, `zdiff3` merge style, fsmonitor for big repos).

## Insight

Reach for this whenever you set up a fresh machine or onboard a junior. The biggest single-character wins are `diff.algorithm = histogram` (vastly better diff output for moved code), `push.autoSetupRemote = true` (no more "set-upstream" gymnastics), `branch.sort = -committerdate` + `column.ui = auto` (sane `git branch` output), and `rebase.autoSquash = true` + `rebase.autoStash = true` (fixup workflow becomes painless). The "matter of taste" block is genuinely subjective — try `merge.conflictstyle = zdiff3` once and decide; some people love the extra base-version context, others find the third marker noisy.

## Similar / related topics

- [[git_delta]] — pretty diff/blame pager that pairs with the `diff.*` settings here.
- GitButler — the blog's authors; alternative branch-management UI on top of Git.
- `git config --global` reference — `git help config` for every key documented here.
- `direnv` / per-repo `.gitconfig` — when global isn't right for one repo.
- conventional-commits / commitlint — if you adopt `commit.verbose = true` you may also want commit-message conventions.

## Internal links

- [[git_delta]] — Companion: better diff/blame rendering, configured exactly via the `core.pager` line in this config.
- [[github]] — Other Git tooling tip in this section.

## Keywords

`#git` `#config` `#gitconfig` `#defaults` `#diff` `#rebase` `#push` `#fetch`

## References / raw notes

Source article: <https://blog.gitbutler.com/how-git-core-devs-configure-git/>

### The full snippet (ready for `~/.gitconfig`)

```ini
[column]
ui = auto
[branch]
sort = -committerdate
[tag]
sort = version:refname
[init]
defaultBranch = main
[diff]
algorithm = histogram
colorMoved = plain
mnemonicPrefix = true
renames = true
[push]
default = simple
autoSetupRemote = true
followTags = true
[fetch]
prune = true
pruneTags = true
all = true

# why the hell not?
[help]
autocorrect = prompt
[commit]
verbose = true
[rerere]
enabled = true
autoupdate = true
[core]
excludesfile = ~/.gitignore
[rebase]
autoSquash = true
autoStash = true
updateRefs = true

# a matter of taste (uncomment if you dare)
[core]
# fsmonitor = true
# untrackedCache = true
[merge]
# (just 'diff3' if git version < 2.3)
# conflictstyle = zdiff3
[pull]
# rebase = true
```

### Per-setting cheat sheet

**Listing**
- `column.ui = auto` — multi-column output for `git branch`, `git status`, `git tag`, `git clean`.
- `branch.sort = -committerdate` — most-recent branches at the top, not alphabetical.
- `tag.sort = version:refname` — natural-version sort (so `0.5.100` comes before `0.5.1000`).

**Init**
- `init.defaultBranch = main` — silences the post-init nag screen.

**Diff**
- `diff.algorithm = histogram` — newer, smarter algorithm; far better at detecting moved/refactored code than the 1986-era default `myers`.
- `diff.colorMoved = plain` — colour moved lines distinctly from added/removed.
- `diff.mnemonicPrefix = true` — replaces `a/`/`b/` in headers with `i/` (index), `w/` (worktree), `c/` (commit).
- `diff.renames = true` — detect file renames in diffs (slightly slower, almost always wanted).

**Push**
- `push.default = simple` — push current branch to same name on remote (default since 2.0).
- `push.autoSetupRemote = true` — no more "fatal: no upstream branch" — just sets it.
- `push.followTags = true` — push local tags along with commits automatically.

**Fetch**
- `fetch.prune = true` — delete `origin/foo` when `foo` is gone on the server.
- `fetch.pruneTags = true` — same for tags.
- `fetch.all = true` — fetch from every configured remote.

**Why the hell not**
- `help.autocorrect = prompt` — Git asks "Did you mean `git status`?" before running it.
- `commit.verbose = true` — the full diff is shown in the commit-message editor for context (stripped on save).
- `rerere.enabled = true` + `rerere.autoupdate = true` — REuse REcorded REsolutions; remembers how you solved a rebase conflict and replays it next time.
- `core.excludesfile = ~/.gitignore` — guessable path for the global ignore (Git also reads `~/.config/git/ignore`).
- `rebase.autoSquash = true` — `git commit --fixup=…` commits get squashed automatically on rebase.
- `rebase.autoStash = true` — uncommitted work is stashed/restored across rebases.
- `rebase.updateRefs = true` — keeps stacked branch refs in sync when the base is rebased.

**Matter of taste (commented out by default)**
- `core.fsmonitor = true` + `core.untrackedCache = true` — speeds up `git status` on huge repos via a per-repo file-watcher process.
- `merge.conflictstyle = zdiff3` — adds a `|||||||` block in conflicts showing the common ancestor. Either super useful or noisy depending on your taste. Requires Git ≥ 2.35.
- `pull.rebase = true` — `git pull` always rebases instead of merging.

### Original article context

Felipe Contreras's "Spring Cleaning" thread on the Git mailing list challenged core developers to remove all their accumulated `gitconfig` and aliases and use vanilla Git. The condensed list of settings most participants ended up re-adding became the basis for this article:

```
merge.conflictstyle = zdiff3
rebase.autosquash = true
rebase.autostash = true
commit.verbose = true
diff.colorMoved = true
diff.algorithm = histogram
grep.patternType = perl
feature.experimental = true
branch.sort = committerdate
```
