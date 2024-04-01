# Bash notes

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

## The `xargs` command

Some commands won't accept `stdin`. Others will behave differently when receiving `stdin` than when receiving parameters. With `xargs` we can control this. `xargs` turns `stdin` into a bunch of parameters for the next command.

For example, suppose that in a directory we only have two files: `hi4.txt` (4 lines long) and `bye5.txt` (5 lines long). If we do:
* `ls | wc -l` we get `2`, because `wc -l` only sees 2 lines in the `stdin` (`bye5.txt` and `hi4.txt`). This output would have been almost the same if we had some file named `foo` with the content `bye5.txt\nhi4.txt`. `wc -l` sees only 2 lines, doesn't try to search for those two files.
* `ls | xargs wc -l` we get:

```
5 bye5.txt
4 hi4.txt
9 total
```

We get *exactly* the same if we do `wc -l bye5.txt hi4.txt`. `xargs` is doing the conversion for us, programatically.

One relatively useful example: `find ( -type f ) -and ( someotherfilter ) | xargs grep somepattern`. This finds all the files in the cwd and "downwards" recursively that fit some criteria and pipes them as arguments to `grep`. `grep` does have the option `-r` to do recursive, so you could do `grep -r somepattern *`, but you might be passing too many files that way and getting a huge output if the pattern is present in a lot of files.

**NOTE** since linux accepts filenames with spaces and even newline characters, a more robust version of the example is: `find ( -type f ) -and ( someotherfilter ) -print0 | xargs --null grep somepattern`. The `-print0` sets the output of `find` to be separated by the ASCII null character, as opposed to a newline. The `--null` option in `xargs` is used so that `xargs` understands such input.
