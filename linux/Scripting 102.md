# Scripting 102

## Control Flow & Conditionals

| Concept            | Description                                                                   |
| :----------------- | :---------------------------------------------------------------------------- |
| `test`             | Evaluates conditions (exit status 0 indicates true, non-zero indicates false) |
| `[ ]`              | POSIX-compliant test command (requires spaces inside brackets)                |
| `[[ ]]`            | Extended test command (Bash-specific; supports regex and pattern matching)    |
| `if / elif / else` | Conditional decision-making structure                                         |
| `for`              | Iterates over lists, ranges, or file patterns                                 |
| `while`            | Executes a block as long as the condition evaluates to true                   |
| `until`            | Executes a block until the condition evaluates to true                        |
| `&&`, `\|\|`       | Logical AND / OR operators for short-circuit execution                        |

## Special Variables

| Variable      | Description                                                                |
| :------------ | :------------------------------------------------------------------------- |
| `$1`, `$2`, … | Positional parameters passed to a script or function                       |
| `$0`          | Name of the script being executed                                          |
| `$#`          | Total number of positional parameters passed                               |
| `$@`          | All positional parameters (expands as separate words when quoted: `"$@"`)  |
| `$*`          | All positional parameters (joins into a single string when quoted: `"$*"`) |
| `$?`          | Exit status of the last executed command (0 = success)                     |
| `$$`          | Process ID (PID) of the current script or shell                            |

## Input Handling & Shell Builtins

| Command / Builtin | Description                                                                           |
| :---------------- | :------------------------------------------------------------------------------------ |
| `shift`           | Shifts positional parameters to the left (`$2` becomes `$1`, `$3` becomes `$2`, etc.) |
| `read`            | Reads input from standard input or a file into one or more variables                  |
| `source` / `.`    | Executes commands from a file within the current shell context                        |
| `getopts`         | Builtin command used to parse short command-line flags (e.g., `-a`, `-f`)             |
| `shopt`           | Builtin command used to view and modify optional shell behavior settings              |
