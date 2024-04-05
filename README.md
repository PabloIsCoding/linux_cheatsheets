# linux_cheatsheets

This is a poor man's solution to my lack of memory. They are just CSV files that I then format with the `column` command in bash.

## Usage

Add to your `.bashrc` the following lines: 
```
alias fs="column -ts ';' -W 2 -W 3 ~/linux_cheatsheets/filesystem.csv"
alias commands="column -ts ';' -W 2 ~/linux_cheatsheets/commands.csv"
alias wildcards="column -ts ';' -W 2 ~/linux_cheatsheets/wildcards.csv"
alias octal="column -ts ';' -W 2 -W 3 ~/linux_cheatsheets/octal.csv"
```
With this you can type `fs`, `commands` or `wildcards` at any point in the shell and the selected cheatsheet will display in a nice format.


## TODO
* Actually remove the filesystem cheatsheet in favor of a program (for example `fs_help /usr/bin` would explain what the directory `/usr/bin/` is about). This is the rich man's solution.
* Revise the `text_command.csv` with examples instead of being so abstract. Use ch27 of TLCM to do this.
