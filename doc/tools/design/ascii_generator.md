---
title: "ASCII art generator (convertcase.net)"
main_link: https://convertcase.net/ascii-art-generator/
keywords: [ascii-art, ascii-generator, banner, figlet, web-tool, design]
status: reviewed
review_date: 2026/05/03
---

# ASCII art generator (convertcase.net)

**Main link:** <https://convertcase.net/ascii-art-generator/>

## Summary

A free web-based ASCII-art text generator. Pick a font (FIGlet-style — Standard, Slant, Big, Doom, Banner, etc.), type your text, copy the output. No install, no signup, just paste and ship.

## Insight

I keep this bookmarked for the same reason I keep a screenshot tool installed: it's a 5-second job that I don't want to think about. Use it for:

- README banner headers.
- CLI splash screens (`println!` of the FIGlet output).
- Commit-message decorations when you want a colleague to laugh.
- Slide deck section dividers in [[marp]] / Markdown decks.

If you'd rather do it offline or in a script, install **`figlet`** (`brew install figlet` / `apt install figlet`) or its prettier cousin **`toilet`**, or in Rust use the `figlet-rs` / `figrs` crates. But for one-off use the website is faster than remembering which font you wanted.

## Similar / related topics

- [FIGlet](http://www.figlet.org/) — the original CLI; same fonts, scriptable.
- [TOIlet](http://caca.zoy.org/wiki/toilet) — FIGlet with colours and Unicode.
- [patorjk.com/software/taag](https://patorjk.com/software/taag/) — another browser-based generator with a much bigger font catalogue.
- [boxes](https://boxes.thomasjensen.com/) — wraps text in ASCII boxes (good with FIGlet output).
- [[fonts]] — when ASCII isn't enough and you need real typography.

## Internal links

- [[marp]] — drop ASCII banners into Markdown slide decks.
- [[fonts]] — for when you need real typefaces.
- [[mdfried]] — preview the README that contains your banner.

## Keywords

`#ascii-art` `#ascii-generator` `#banner` `#figlet` `#web-tool` `#design` `#tools`

## References / raw notes

<https://convertcase.net/ascii-art-generator/>
