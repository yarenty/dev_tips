---
title: "sshpass: non-interactive SSH password auth (use sparingly)"
main_link: https://sourceforge.net/projects/sshpass/
keywords: [sshpass, ssh, scp, expect, automation, security, shell]
status: reviewed
review_date: 2026/05/03
---

# sshpass: non-interactive SSH password auth (use sparingly)

**Main link:** <https://sourceforge.net/projects/sshpass/>

Manual: <https://linux.die.net/man/1/sshpass>

## Summary

`sshpass` is a tiny utility that feeds a password to `ssh` (or `scp`,
`rsync`, anything that uses ssh under the hood) on a pseudo-terminal so the
SSH client thinks a human typed it. It exists because OpenSSH deliberately
refuses to read passwords from stdin or environment variables — and there
are still situations (lab gear, embedded boards, legacy boxes) where that
constraint hurts.

## Insight

**Use SSH keys instead.** Almost every time you reach for `sshpass` the
right answer is `ssh-copy-id user@host` once, then never deal with passwords
again. Keys are more secure, work with `ssh-agent`, can be rotated, and
don't end up in shell history.

When `sshpass` is genuinely the right tool:

- Bootstrapping a fresh device that doesn't have your key on it yet
  (`ssh-copy-id` itself uses sshpass-style flow internally).
- Scripting against appliances / IoT / network gear that only allow
  password auth and where you can't install a key.
- One-shot test harnesses where the "password" is a known-throwaway value.

Hard rules if you have to use it:

- **Never** use `-p PASSWORD` on the command line — it shows up in `ps`
  output and shell history. Use `-f passwordfile` (mode 600) or
  `-e` with `SSHPASS` env var.
- Treat any host where you used sshpass as compromised for the purpose of
  that password — assume it leaked.
- For SCP/SFTP automation, prefer `rsync` over SSH with key auth; for
  expect-style scripted dialogues, `expect` itself (see below) is more
  flexible.

## Similar / related topics

- `ssh-copy-id` — the right answer 95% of the time; copies your public key once.
- [`expect`](https://core.tcl-lang.org/expect/) — the general TCL-based
  scripted-dialogue tool; more flexible, more verbose.
- [`sshfs`](https://github.com/libfuse/sshfs) — mount a remote dir over SSH
  instead of scripting `scp` calls.
- [`rsync`](https://rsync.samba.org/) — almost always a better fit for
  copying files over SSH.
- [[mosh]] — interactive SSH replacement that survives roaming and sleep.

## Internal links

- [[mosh]]
- [[tools/shell/tools|shell tools]]
- [[must_have]]
- [[dns_toys]]

## Keywords

`#sshpass` `#ssh` `#scp` `#expect` `#automation` `#security` `#shell`

## References / raw notes

Install:

```shell
# Debian / Ubuntu
sudo apt install sshpass

# macOS via Homebrew (sshpass was removed from core; use the formula below)
brew install hudochenkov/sshpass/sshpass
```

### One-liners (use a password file, not `-p`)

Inline (NOT RECOMMENDED — leaks via `ps`):

```shell
sshpass -p "password" scp -r user@example.com:/some/remote/path /some/local/path
```

From a 600-mode file (preferred):

```shell
sshpass -f /path/to/passwordfile scp -r user@example.com:/some/remote/path /some/local/path
```

From an environment variable:

```shell
SSHPASS='secret' sshpass -e ssh user@example.com 'uname -a'
```

### Replacement using `expect`

When `sshpass` isn't installed and you can't add it, `expect` does the same
job. Save as `test.exp`:

```expect
#!/usr/bin/expect
spawn scp /usr/bin/file.txt root@<ServerLocation>:/home
set pass "Your_Password"
expect {
    password: { send "$pass\r"; exp_continue }
}
```

Run:

```shell
expect test.exp
```
