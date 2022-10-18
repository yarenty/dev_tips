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
