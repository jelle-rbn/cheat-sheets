# Scripting 201 - Job Scheduling & Process Management

## Advanced Shell & Functions

| Command / Construct                        | Description                                                     |
| :----------------------------------------- | :-------------------------------------------------------------- |
| `name() { ... }` / `function name { ... }` | Defines a shell function                                        |
| `$1`, `$2`, ...                            | Positional parameters passed into a function                    |
| `$#`                                       | Number of arguments passed to a function                        |
| `$@`                                       | All function arguments as individual words                      |
| `local var=value`                          | Declares a variable scoped locally to the function              |
| `return N`                                 | Exits a function with a status code `N` (0-255)                 |
| `$(...)`                                   | Command substitution (captures output of a command or function) |
| `(( ))`                                    | Arithmetic evaluation and numeric comparison                    |
| `let`                                      | Legacy arithmetic command; supports base conversion             |
| `case`                                     | Pattern-matching construct for multi-branch logic               |
| `eval`                                     | Evaluates and executes a string as a shell command              |
| `read`                                     | Prompt and store user input into variables                      |
| `echo`                                     | Output text or variable values                                  |

### Text & Utility Commands

| Command              | Description                                                    |
| :------------------- | :------------------------------------------------------------- |
| `getent passwd USER` | Queries user account details (check existence via exit status) |
| `shuf`               | Randomly shuffles lines of input                               |
| `head -n N`          | Displays the first `N` lines                                   |
| `tr '\n' ' '`        | Replaces all newline characters with spaces                    |

## Process Control & Background Jobs

| Command / Shortcut | Description                                                   |
| :----------------- | :------------------------------------------------------------ |
| `&`                | Appended to a command to run it in the background immediately |
| `Ctrl+Z`           | Suspends (pauses) the current foreground process (`SIGSTOP`)  |
| `jobs`             | Lists active background jobs in the current shell             |
| `jobs -p`          | Lists process IDs (PIDs) of background jobs                   |
| `fg [%N]`          | Brings a background/suspended job to the foreground           |
| `bg [%N]`          | Resumes a suspended job in the background (`SIGCONT`)         |
| `ps`               | Displays process information (used to find job PIDs)          |

## Job Scheduling

### Cron Syntax Quick Reference

```text
* * * * *  command_to_execute
│ │ │ │ │
│ │ │ │ └─ Day of week (0 - 6) (Sunday = 0 or 7)
│ │ │ └─── Month (1 - 12)
│ │ └───── Day of month (1 - 31)
│ └─────── Hour (0 - 23)
└───────── Minute (0 - 59)
```
