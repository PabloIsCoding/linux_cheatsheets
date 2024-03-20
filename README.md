# linux_cheatsheets

This is a poor man's solution to my lack of memory. They are just CSV files that I then format with the `column` command in bash.

# Usage

Add to your `.bashrc` an alias like:
`alias fs="column -ts ';' -W 2 -W 3 ~/linux_cheatsheets/filesystem.csv"`
With this you can type `fs` at any point in the shell and the cheatsheet will display in a nice format.

# TODO
* Add a cheatsheet for *useful* command options that I often forget
* Actually remove the filesystem cheatsheet in favor of a program (for example `fs_help /usr/bin` would explain what the directory `/usr/bin/` is about). This is the rich man's solution.
