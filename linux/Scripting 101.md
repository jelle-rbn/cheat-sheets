# Scripting 101

## I/O Redirection

| Task                                                                      | Command / Syntax                        |
| :------------------------------------------------------------------------ | :-------------------------------------- |
| Prevent overwriting existing files (enable _noclobber_)                   | `set -o noclobber`                      |
| Allow overwriting files again (disable _noclobber_)                       | `set +o noclobber`                      |
| Override _noclobber_ for a single command                                 | `command > \| <file>`                   |
| Redirect standard output (`stdout`) to a file (overwrites file)           | `command > <file>`                      |
| Append standard output (`stdout`) to a file                               | `command >> <file>`                     |
| Redirect standard error (`stderr`) to a file                              | `command 2> <file>`                     |
| Redirect both `stdout` and `stderr` to the same file (classic syntax)     | `find / > allfiles_and_errors.txt 2>&1` |
| Merge `stdout` and `stderr` into a single stream to a file (short syntax) | `command &> <file>`                     |
| Read standard input (`stdin`) from a file                                 | `command < <file>`                      |

## Filters

| Task                                                                   | Command                                                 |
| :--------------------------------------------------------------------- | :------------------------------------------------------ |
| Display the contents of a file                                         | `cat <file>`                                            |
| Display the lines of a file in reverse order                           | `tac <file>`                                            |
| Display the lines of a file in random order                            | `shuf <file>`                                           |
| Copy `stdin` to `stdout` and also write it to a file                   | `cal                                     \| tee <file>` |
| Display the first lines of a file (default = 10)                       | `head -n 3 <file>`                                      |
| Display the last lines of a file (default = 10)                        | `tail -n 3 <file>`                                      |
| Display lines starting from line number N                              | `tail -n +3 <file>`                                     |
| Display all lines except the last N lines                              | `head -n -3 <file>`                                     |
| Display the last lines and wait for new data to be appended            | `sudo tail -f /var/log/httpd/access_log`                |
| Select columns using a delimiter (`-d`) and field numbers (`-f`)       | `cut -d: -f1,3 /etc/passwd               \| tail -4`    |
| Use a space as the delimiter                                           | `cut -d' ' -f1 <file>`                                  |
| Select only specific character positions (`-c`)                        | `cut -c2-7 /etc/passwd                   \| tail -4`    |
| Combine two files line by line (default delimiter = tab)               | `paste -d',' <file1> <file2>`                           |
| Join two files on a common field (`-t` specifies delimiter)            | `join -t, <file1> <file2>`                              |
| Sort lines alphabetically                                              | `sort <file>`                                           |
| Remove duplicate lines from a sorted list                              | `sort <file>                             \| uniq`       |
| Count occurrences of unique lines (`-c`)                               | `sort <file>                             \| uniq -c`    |
| Reformat text to a specified width without breaking words              | `fmt -w 35 <file>`                                      |
| Add line numbers to text                                               | `nl <file>`                                             |
| Count lines, words, and characters                                     | `wc <file>`                                             |
| Align output into clean columns                                        | `column -t -s: <file>`                                  |
| Compare two sorted files line by line                                  | `comm <file1> <file2>`                                  |
| Compare two files and suppress unique lines (`-12` shows matches only) | `comm -12 <file1> <file2>`                              |
| Filter lines matching a text pattern                                   | `grep <pattern> <file>`                                 |
| Filter lines using case-insensitive matching                           | `grep -i <pattern> <file>`                              |
| Invert search: output lines _not_ matching the string                  | `grep -v <pattern> <file>`                              |
| Output one line _after_ each match                                     | `grep -A1 <pattern> <file>`                             |
| Output one line _before_ each match                                    | `grep -B1 <pattern> <file>`                             |
| Output one line _before_ and _after_ each match                        | `grep -C1 <pattern> <file>`                             |
| Translate or replace single characters                                 | `tr 'e' 'E' < <file>`                                   |
| Convert all lowercase letters to uppercase                             | `tr 'a-z' 'A-Z' < <file>`                               |
| Replace all newline characters with spaces                             | `tr '\n' ' ' < <file>`                                  |
| Squeeze multiple consecutive occurrences of a character into one       | `tr -s ' ' < <file>`                                    |
| Obfuscate/de-obfuscate text using ROT13 encoding                       | `tr 'a-zA-Z' 'n-za-mN-ZA-M' < <file>`                   |
| Delete specified characters                                            | `tr -d 'e' < <file>`                                    |
| Delete empty lines (`^$` = empty line)                                 | `sed '/^$/d' <file>`                                    |
| Substitute the first occurrence of a pattern per line                  | `sed 's/5/42/' <file>`                                  |
| Substitute the 2nd occurrence of a pattern per line                    | `sed 's/5/42/2' <file>`                                 |
| Substitute all occurrences of a pattern per line (global)              | `sed 's/5/42/g' <file>`                                 |
| Delete lines matching a regular expression from stream                 | `sed '/<pattern>/d' <file>`                             |
| View file contents or stream output one page at a time                 | `less <file>`                                           |

## Practical Pipeline Examples

| Task                                                  | Command                                                    |
| :---------------------------------------------------- | :--------------------------------------------------------- |
| Count how many user sessions are logged on            | `who                      \| wc -l`                        |
| Display a sorted list of logged-on users              | `who                      \| awk '{print $1}'  \| sort`    |
| Display a sorted list of unique logged-on users       | `who                      \| awk '{print $1}'  \| sort -u` |
| Display a list of all user accounts on the system     | `cut -d: -f1 /etc/passwd`                                  |
| Display only the IPv4 addresses of network interfaces | `ip -br a                 \| awk '{print $3}'`             |

## Regular Expressions (Regex)

### Common Metacharacters

| Character | Description                                                 | Example                    |
| :-------- | :---------------------------------------------------------- | :------------------------- |
| `^`       | Matches the beginning of a line                             | `grep '^root' /etc/passwd` |
| `$`       | Matches the end of a line                                   | `grep 'bash$' /etc/passwd` |
| `.`       | Matches any single character                                | `grep 'r..t' /etc/passwd`  |
| `*`       | Matches zero or more occurrences of the preceding character | `grep 'ho*me' <file>`      |
| `[ ]`     | Matches any single character within the brackets            | `grep '[bB]ash' <file>`    |
| `[^ ]`    | Matches any character _not_ listed within the brackets      | `grep '[^0-9]' <file>`     |
| `[a-z]`   | Matches any character in the range `a` to `z`               | `grep '[0-9]' <file>`      |
| `\`       | Escapes a special character to treat it literally           | `grep '\.' <file>`         |

### Handy Regex Commands & Patterns

| Task                                                          | Command                                                                                                   |
| :------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------- |
| Search for empty lines                                        | `grep '^$' <file>`                                                                                        |
| Search for non-empty lines                                    | `grep -v '^$' <file>`                                                                                     |
| Filter out comment lines (lines starting with `#`)            | `grep -v '^#' /etc/ssh/sshd_config`                                                                       |
| Filter out both empty lines and comments                      | `grep -E -v '^(#                                                     \| $)' <file>`                       |
| Match whole words only                                        | `grep -w 'user' <file>`                                                                                   |
| Extended Regex (ERE): Search for multiple terms (OR operator) | `grep -E 'root                                                       \| admin       \| sudo' /etc/passwd` |
| Match a specific number of repetitions (e.g., 3 to 5 digits)  | `grep -E '[0-9]{3,5}' <file>`                                                                             |
| Extract IP addresses using regex matching                     | `grep -E -o '([0-9]{1,3}\.){3}[0-9]{1,3}' <file>`                                                         |
| Extract email addresses                                       | `grep -E -o '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' <file>`                                      |
| Strip HTML/XML tags from a stream                             | `sed 's/<[^>]*>//g' <file>`                                                                               |
| Remove leading whitespace from each line                      | `sed 's/^[ \t]*//' <file>`                                                                                |
| Remove trailing whitespace from each line                     | `sed 's/[ \t]*$//' <file>`                                                                                |
