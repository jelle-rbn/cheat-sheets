# Vim Cheat Sheet

## Mode

| Command                      | Use                                                       |
| ---------------------------- | --------------------------------------------------------- |
| `<ESC>`                      | Exit to the main mode.                                    |
| `i`                          | Enable _insert_ mode (write and edit text).               |
| `R`                          | Enable _replace_ mode (replace text under the cursor).    |
| `v`                          | Enable _visual_ mode (select text using cursor or mouse). |
| `v` + Motion + `:w filename` | Select, then save selection as _filename_.                |

## Navigate

| Command       | Use                                                         |
| ------------- | ----------------------------------------------------------- |
| `j`           | Move one line down.                                         |
| `k`           | Move one line up.                                           |
| `h`           | Move one character left.                                    |
| `l`           | Move one character right.                                   |
| `w`           | Move to the start of the next word.                         |
| `e`           | Move to the end of the word.                                |
| `b`           | Move to the beginning of the word.                          |
| `0` of `^`    | Move to the start of the line.                              |
| `$`           | Move to the end of the line.                                |
| `Ctrl-f`      | Move one page forward.                                      |
| `Ctrl-b`      | Move one page backward.                                     |
| `gg` of `[[`  | Move to the start of the file.                              |
| `G` of `]]`   | Move to the end of the file.                                |
| `:N` of `N G` | Move to the _N_-th line.                                    |
| `%`           | Move to the matching parenthesis or bracket.                |
| `Ctrl-o`      | Move to the previous cursor position in the file.           |
| `Ctrl-i`      | Move forward between previous cursor positions in the file. |

## Edit

| Command  | Use                                     |
| -------- | --------------------------------------- |
| `u`      | Undo changes.                           |
| `U`      | Undo all changes on line.               |
| `Ctrl-r` | Redo changes.                           |
| `x`      | Delete character.                       |
| `X`      | Delete character backwards.             |
| `y`      | Copy selected text.                     |
| `yy`     | Copy a line.                            |
| `y3w`    | Copy three words.                       |
| `d`      | Delete selected text.                   |
| `d2d`    | Delete two lines.                       |
| `dw`     | Delete a word.                          |
| `d0`     | Delete to start of line.                |
| `d$`     | Delete to end of line.                  |
| `dgg`    | Delete to start of file.                |
| `p`      | Paste _below_ the current line.         |
| `P`      | Paste _above_ the current line.         |
| `r`      | Replace the character under the cursor. |
| `o`      | Insert new line below.                  |
| `O`      | Insert new line above.                  |

## Search

| Command      | Use                                                   |
| ------------ | ----------------------------------------------------- |
| `/term`      | Search _term_ forward.                                |
| `?term`      | Search _term_ backward.                               |
| `n`          | Search for the same _term_ again.                     |
| `N`          | Search for the same _term_ in the opposite direction. |
| `:set ic`    | Ignore case (not case-sensitive).                     |
| `:set noic`  | Case-sensitive search.                                |
| `:set hls`   | Highlight matches.                                    |
| `:set nohls` | Do not highlight matches.                             |

## Substitute / Replace

| Command           | Use                                                              |
| ----------------- | ---------------------------------------------------------------- |
| `:s/from/to`      | Replace the first occurrence of _from_ on the line.              |
| `:N,Ms/from/to/g` | Replace all occurrences of _from_ between lines _N_ and _M_.     |
| `:%s/from/to`     | Replace the first occurrence of _from_ on each line of the file. |
| `:%s/from/to/g`   | Replace all occurrences of _from_ in the file.                   |
| `:%s/from/to/gc`  | Replace all occurrences of _from_ asking for confirmation.       |

## Patterns

| Command                    | Use                                                           |
| -------------------------- | ------------------------------------------------------------- |
| `[Number] Operator`        | Execute _Operator_ action a _Number_ of times.                |
| `3j`                       | Move three lines down.                                        |
| `2x`                       | Delete 2 characters.                                          |
| `Operator [Number] Motion` | Execute _Operator_ action a _Number_ of times using _Motion_. |
| `d$`                       | Delete to the end of the line.                                |
| `dgg`                      | Delete to the start of the file.                              |
| `d2w`                      | Delete 2 words.                                               |
| `d4d`                      | Delete 4 lines.                                               |

## Exit

| Command       | Use                                              |
| ------------- | ------------------------------------------------ |
| `:q!`         | Close file without saving.                       |
| `:qa!`        | Close all files without saving.                  |
| `:wq`         | Save changes and close file.                     |
| `:w filename` | Write the current file to disk using _filename_. |

## Commands

| Command         | Use                                  |
| --------------- | ------------------------------------ |
| `:set nu`       | Show line numbers.                   |
| `:set nonu`     | Hide line numbers.                   |
| `:syntax on`    | Enable syntax highlighting.          |
| `:syntax off`   | Disable syntax highlighting.         |
| `:set list`     | Show newline characters.             |
| `:set nolist`   | Hide newline characters.             |
| `:!command`     | Execute an external shell _command_. |
| `:!ls`          | List current directory contents.     |
| `:!rm filename` | Remove _filename_.                   |
| `:r filename`   | Insert contents of _filename_.       |
| `:r !command`   | Insert output of a shell _command_.  |

## Help

| Command           | Use                                 |
| ----------------- | ----------------------------------- |
| `vimtutor`        | Open vim tutorial in your terminal. |
| `:help` of `<F1>` | Show help.                          |
