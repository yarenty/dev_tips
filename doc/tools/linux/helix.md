# Helix
My Neovim config had 21 external plugins. Making LSP, tree-sitter and formatting work took a while (LSP alone needs 3 plugins) and in the end there were still things that didn’t work.

I’ve switched to Helix, which can do so much out of the box, here’s a non-exhaustive list:

LSP (including autocompletion, show signature, go to definition, show references, etc.) just works
Tree-sitter is built in, you can even do selections on tree-sitter objects
A file picker and global search
Pressing a key in normal mode shows subsequent keys you can press, and what they do
You can jump to any visible word, add/remove/replace quotes or other characters
… and so much more
The config for the code editor I use all day is 5 loc. Here it is:

theme = "kanagawa"

[editor]
line-number = "relative"
cursorline = true
rulers = [80]
I will say that it takes some getting used to as it folows the selection -> action model, i.e. you need to run wd instead of dw to delete the next word.

