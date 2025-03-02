# delta


brew install git-delta


https://github.com/dandavison/delta


https://dandavison.github.io/delta/installation.html


```bash
git config --global core.pager delta
git config --global interactive.diffFilter 'delta --color-only'
git config --global delta.navigate true
git config --global merge.conflictStyle zdiff3
```