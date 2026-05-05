---
title: Shell tools
main_link: https://github.com/wfxr/forgit
keywords: [shell, prompt, gum, forgit, wfxr, git]
status: draft
---

<!-- auto-stubbed by article_stub.py -->
<!-- keywords-extended by P6.5 -->

# Shell tools

**Main link:** <https://github.com/wfxr/forgit>

## Summary

<!-- TODO: 2-5 sentences. What is this? Who made it? What does it do? -->

## Insight

<!-- TODO: Why care? When and where to reach for this? Gotchas, opinions, comparisons. -->

## Similar / related topics

<!-- TODO: 3-5 bullets, each "name — 1-line description". -->

## Internal links

<!-- internal-links-suggested by P6.3 -->
> Auto-suggested by P6.3. Review, prune, and replace this comment with `<!-- reviewed -->` once curated.

- [[must_have]] — Commands to install _(score 27.5)_
- [[sshpass]] — sshpass _(score 21.4)_
- [[tools/shell/tmux|tmux]] — Tmux _(score 21.4)_
- [[lynx]] — Lynx _(score 21.4)_
- [[fish]] — Fish _(score 17.4)_

<!-- TODO: review the auto-suggested links above; remove low-signal ones, add ones P6.3 missed. -->
## Keywords

`#tools` `#shell` `#prompt` `#gum` `#forgit` `#wfxr`

## TODO

- Write a real `## Summary` (2-5 sentences) replacing the auto-stub placeholder.
- Write a real `## Insight` (when/why/where to use) replacing the auto-stub placeholder.
- Add 3-5 entries under `## Similar / related topics`.
- Add `[[wikilinks]]` to at least 2 related articles in the vault under `## Internal links`.
- Promote `status: draft` to `status: reviewed` once the rewrite is complete.

## References / raw notes

<!-- Original content preserved verbatim below. Curate / prune during rewrite. -->

# Shell tools 

## forgit

https://github.com/wfxr/forgit

Utility tool for using git interactively. Powered by junegunn/fzf.
This tool is designed to help you use git more efficiently. It's lightweight and easy to use.


### for fisher
fisher install wfxr/forgit

### for omf
omf install https://github.com/wfxr/forgit



## GUM

https://github.com/charmbracelet/gum


A tool for glamorous shell scripts. Leverage the power of Bubbles and Lip Gloss in your scripts and aliases without writing any Go code!


Start with a #!/bin/sh.
```shell
#!/bin/sh
```

Ask for the commit type with gum choose:
```shell
gum choose "fix" "feat" "docs" "style" "refactor" "test" "chore" "revert"
```
Tip: this command itself will print to stdout which is not all that useful. To make use of the command later on you can save the stdout to a $VARIABLE or file.txt.

Prompt for an (optional) scope for the commit:
```shell
gum input --placeholder "scope"
```
Prompt for a commit message:
```shell
gum input --placeholder "Summary of this change"
```
Prompt for a detailed (multi-line) explanation of the changes:

```shell
gum write --placeholder "Details of this change"
```

Prompt for a confirmation before committing:

gum confirm exits with status 0 if confirmed and status 1 if cancelled.

```shell
gum confirm "Commit changes?" && git commit -m "$SUMMARY" -m "$DESCRIPTION"
```
Putting it all together...

```shell
#!/bin/sh
TYPE=$(gum choose "fix" "feat" "docs" "style" "refactor" "test" "chore" "revert")
SCOPE=$(gum input --placeholder "scope")

# Since the scope is optional, wrap it in parentheses if it has a value.
[[ -n "$SCOPE" ]] && SCOPE="($SCOPE)"

# Pre-populate the input with the type(scope): so that the user may change it
SUMMARY=$(gum input --value "$TYPE$SCOPE: " --placeholder "Summary of this change")
DESCRIPTION=$(gum write --placeholder "Details of this change")

# Commit these changes
gum confirm "Commit changes?" && git commit -m "$SUMMARY" -m "$DESCRIPTION"
Running the ./examples/commit.sh script to commit to git
```
