# IO redirection notes

## Redirection operators (`>`, `>>`)

Redirect stdout to a file: `> myfile`. Example `ls > out.txt`
Redirect stderr to a file: `2> myfile`. Example `ls non_existent_dir 2> error.txt` will write the following into error.txt: `ls: cannot access 'non_existent_dir': No such file or directory`

**IMPORTANT** both `>` and `2>` create a new file or **overwrite the existing one completely**.

Redirect stdout to a file, but **appending to it, instead of overwriting**: `>> myfile`. Example `ls >> out.txt`
Redirect stderr to a file but **appending to it, instead of overwriting**: `2>> myfile`. Example `ls non_existent_dir 2>> error.txt`

---
Why is it `2>` for stderr?

* Standard input = 0
* Standard output = 1
* Standard error = 2

So for example, typing `1>` is the same as `>`, and `1>>` is equivalent to `>>`.

---

Redirect both stdout and stderr to the same file:
* Traditional/fully compatible way: `> somefile 2>&1`
* Bash-specific way: `&> somefile`


Examples:
* `ls > general_output.txt 2>&1`
* `ls &> general_output.txt`

Of course, we can also use `>>`, except in the traditional style. For example, `ls >> general_output.txt 2>>&1` does not work (even though `ls >> general_output.txt 2>&1` does work). TODO: figure out how to do this in the traditional style, probably is a problem of operator precedence or similar.

To discard outputs of the program, redirect to `/dev/null` (which is not really a directory). For example `ls somedir 2> /dev/null`.

## Pipeline operator (`|`)

While the redirection operators `>`and `>>` send stdout/stderr to a file, with the pipe operator we can send it to the stdin of another command.

For example, we don't do `ls > less` (which would create a file named `less`), we do `ls | less` because `less` is a command.

Some useful commands to pipe into are `sort`, `uniq`, `wc`, `tee`.

## The `tee` command

There are cases where you want to redirect stdout both to a file and down the pipe. `tee` does this easily. Example: `ls | tee ls.txt | wc -l`. This will write the output of ls into `ls.txt` and also pipe it to `wc -l` to count the number of lines and print it into the terminal.

