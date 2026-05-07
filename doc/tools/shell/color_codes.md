---
title: "Shell color codes: ANSI escape cheat sheet"
main_link: https://en.wikipedia.org/wiki/ANSI_escape_code
keywords: [ansi, color-codes, escape-codes, terminal, shell, rust, colored, truecolor]
status: reviewed
---

# Shell color codes: ANSI escape cheat sheet

**Main link:** <https://en.wikipedia.org/wiki/ANSI_escape_code>

Reference: <https://gist.github.com/JBlond/2fea43a3049b38287e5e9cefc87b2124> · Rust crate: <https://docs.rs/colored>

## Summary

This is a cheat sheet for the ANSI/VT100 escape codes that change foreground
colour, background colour, and text style in any reasonable terminal. The
escapes are just bytes: `ESC [` (`\x1b[` / `\033[`) followed by
semicolon-separated parameters terminated by `m`. Same codes work in `bash`,
`zsh`, [[fish]], `printf`, Python `print`, Rust, Go, anything that writes to
a TTY.

## Insight

For one-off scripts the codes-as-strings approach below is fine. For anything
bigger, reach for a library that knows about `NO_COLOR`, `--no-color`,
`isatty()`, and Windows console quirks: `colored` in Rust, `chalk` in
Node, `rich` in Python, `lipgloss`/`gum` in Go (see [[tools/shell/tools|shell tools]]).

Two gotchas that catch people every time:

- **Always reset.** Forget the `\033[0m` reset and the colour bleeds into
  every subsequent line, including the prompt after your script exits.
- **`echo` vs `printf`.** Plain `echo "\033[31mhi\033[0m"` prints the
  literal escape on most shells. Use `echo -e` (bash) or `printf` (portable):
  `printf '\033[31mhi\033[0m\n'`.
- **256-colour and truecolour** use a different syntax —
  `\033[38;5;<N>m` for the 256-palette, `\033[38;2;<R>;<G>;<B>m` for
  24-bit. Most modern terminals support both; check `$COLORTERM=truecolor`.

## Similar / related topics

- [`colored` (Rust crate)](https://docs.rs/colored) — ergonomic ANSI styling for Rust.
- [`gum`](https://github.com/charmbracelet/gum) — see [[tools/shell/tools|shell tools]]; styled prompts and output without escapes.
- [`bat`](https://github.com/sharkdp/bat) — `cat` with syntax-highlighted ANSI output.
- [NO_COLOR spec](https://no-color.org/) — the convention every CLI tool should honour.
- [[fonts]] — pair colour with a Powerline / Nerd Font for prompt glyphs.

## Internal links

- [[tools/shell/tools|shell tools]]
- [[must_have]]
- [[fish]]
- [[fonts]]

## Keywords

`#ansi` `#color-codes` `#escape-codes` `#terminal` `#shell` `#truecolor` `#rust`

## References / raw notes

### Quick reset

```
\x1b[0m
```

### From Rust (with the `colored` crate)

```rust
use colored::Colorize;

fn main() {
    println!(
        "{}, {}, {}, {}, {}, {}, and some normal text.",
        "Bold".bold(),
        "Red".red(),
        "Yellow".yellow(),
        "Green Strikethrough".green().strikethrough(),
        "Blue Underline".blue().underline(),
        "Purple Italics".purple().italic(),
    );
}
```

### Bash variables for the full 8/16-colour palette

```shell
# Reset
Color_Off='\033[0m'       # Text Reset

# Regular Colors
Black='\033[0;30m'
Red='\033[0;31m'
Green='\033[0;32m'
Yellow='\033[0;33m'
Blue='\033[0;34m'
Purple='\033[0;35m'
Cyan='\033[0;36m'
White='\033[0;37m'

# Bold
BBlack='\033[1;30m'
BRed='\033[1;31m'
BGreen='\033[1;32m'
BYellow='\033[1;33m'
BBlue='\033[1;34m'
BPurple='\033[1;35m'
BCyan='\033[1;36m'
BWhite='\033[1;37m'

# Underline
UBlack='\033[4;30m'
URed='\033[4;31m'
UGreen='\033[4;32m'
UYellow='\033[4;33m'
UBlue='\033[4;34m'
UPurple='\033[4;35m'
UCyan='\033[4;36m'
UWhite='\033[4;37m'

# Background
On_Black='\033[40m'
On_Red='\033[41m'
On_Green='\033[42m'
On_Yellow='\033[43m'
On_Blue='\033[44m'
On_Purple='\033[45m'
On_Cyan='\033[46m'
On_White='\033[47m'

# High Intensity
IBlack='\033[0;90m'
IRed='\033[0;91m'
IGreen='\033[0;92m'
IYellow='\033[0;93m'
IBlue='\033[0;94m'
IPurple='\033[0;95m'
ICyan='\033[0;96m'
IWhite='\033[0;97m'

# Bold High Intensity
BIBlack='\033[1;90m'
BIRed='\033[1;91m'
BIGreen='\033[1;92m'
BIYellow='\033[1;93m'
BIBlue='\033[1;94m'
BIPurple='\033[1;95m'
BICyan='\033[1;96m'
BIWhite='\033[1;97m'

# High Intensity backgrounds
On_IBlack='\033[0;100m'
On_IRed='\033[0;101m'
On_IGreen='\033[0;102m'
On_IYellow='\033[0;103m'
On_IBlue='\033[0;104m'
On_IPurple='\033[0;105m'
On_ICyan='\033[0;106m'
On_IWhite='\033[0;107m'
```

### 256-colour and truecolour

```shell
# 256-colour foreground (N = 0..255)
printf '\033[38;5;208mthis is orange-256\033[0m\n'

# 24-bit truecolour foreground (R;G;B)
printf '\033[38;2;255;105;180mhot pink\033[0m\n'
```
