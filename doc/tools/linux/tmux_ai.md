---
title: "TmuxAI — LLM-augmented tmux"
main_link: https://tmuxai.dev/
keywords: [tmuxai, tmux, llm, ai, agent, terminal, automation, copilot]
status: reviewed
review_date: 2026/05/03
---

# TmuxAI — LLM-augmented tmux

**Main link:** <https://tmuxai.dev/>

## Summary

TmuxAI is an "AI agent in your terminal" that runs as a sidecar pane inside a regular tmux session. It watches the contents of your other panes, lets you chat with an LLM in a dedicated chat pane, and — crucially — can *propose and (with confirmation) send keystrokes* to the panes around it. So you can say "start a MySQL container and connect to its shell" and it will type the commands for you, asking before each one.

## Insight

This is a different shape from `gh copilot suggest` or shell completion plugins: TmuxAI sees the live state of your terminal (output, errors, prompts) and can *act*, not just suggest text to copy. That makes it useful for chained tasks — "run the test suite, find the failing test, open it in `$EDITOR`" — and dangerous if you let it run unattended (it asks `[Y]es/No/Edit:` before each action, but a habit-of-yes is risky).

When to reach for it:

- Onboarding to an unfamiliar repo: ask it to install deps and run the test suite while you watch.
- Repetitive multi-step ops (docker exec dance, k8s context juggling) where you'd otherwise alt-tab to a chat window and copy commands back.
- Interactive bug reproduction where you want a record of "what was tried".

When *not* to:

- Anything destructive on production. Always.
- When you want to actually learn the tool you're using — TmuxAI is a productivity tool, not a teacher.

It pairs naturally with [[tools/linux/tmux|tmux]] (obviously), but it also works inside [[zellij]] if you launch tmux inside a zellij pane. You'll need an API key for whichever LLM provider you point it at.

## Similar / related topics

- [[tools/linux/tmux|tmux]] — the multiplexer TmuxAI lives inside.
- [GitHub Copilot CLI / `gh copilot`](https://github.com/github/gh-copilot) — text-suggestion-only counterpart.
- [Aider](https://aider.chat/) — repo-aware AI pair programmer (operates on files, not panes).
- [Warp AI](https://www.warp.dev/) — terminal emulator with AI baked in (GUI, not tmux-based).
- [[helix]] — modal editor; combine TmuxAI with Helix in adjacent panes.

## Internal links

- [[tools/linux/tmux|tmux]]
- [[zellij]]
- [[helix]]
- [[fish]]

## Keywords

`#tmuxai` `#tmux` `#llm` `#ai` `#agent` `#terminal` `#automation` `#copilot`

## References / raw notes

### Source

<https://tmuxai.dev/>

### Example session

```text
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

### Notes

- Each command is gated behind a `[Y]es/No/Edit:` prompt — review carefully before pressing `y`.
- It needs an API key for an LLM provider; do not commit this to a repo.
- Recommended posture: dedicate one tmux window to TmuxAI's chat pane plus the worker pane(s) it controls; keep your editor in a different window so TmuxAI can't accidentally type into it.
