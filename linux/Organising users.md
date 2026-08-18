# Organising Users

## List of Files

| First character | File type                  |
| :-------------- | :------------------------- |
| `-`             | Normal file                |
| `d`             | Directory                  |
| `l`             | Symbolic link              |
| `p`             | Named pipe                 |
| `b`             | Block device (hard disk)   |
| `c`             | Character device (console) |
| `s`             | Socket                     |

## Permissions

| Permission | On a file                  | On a directory                      |
| :--------- | :------------------------- | :---------------------------------- |
| Read       | Read file contents (`cat`) | Read directory contents (`ls`)      |
| Write      | Change file contents       | Create/delete files (`touch`, `rm`) |
| Execute    | Execute the file           | Enter the directory (`cd`)          |

| Position | Characters | Function                          |
| :------- | :--------- | :-------------------------------- |
| 1        | `-`        | File type                         |
| 2-4      | `rwx`      | Permissions for the _user owner_  |
| 5-7      | `r-x`      | Permissions for the _group owner_ |
| 8-10     | `r--`      | Permissions for _others_          |

### Setting Permissions (Symbolic Notation)

Give permissions: `+`  
Remove permissions: `-`  
Set explicit permissions: `=`

| Task                                                 | Command                      |
| :--------------------------------------------------- | :--------------------------- |
| Give (`+`) user owner (`u`) execute (`x`) permission | `chmod u+x <file>`           |
| Remove (`-`) group owner (`g`) read (`r`) permission | `chmod g-r <file>`           |
| Remove (`-`) others (`o`) read (`r`) permission      | `chmod o-r <file>`           |
| Give (`+`) all (`a`) write (`w`) permission          | `chmod a+w <file>`           |
| Set explicit permissions with `=`                    | `chmod u=rw <file>`          |
| Combination of options                               | `chmod u=rw,g=rw,o=r <file>` |

### Setting Permissions (Octal Notation)

| Permission | Binary | Octal |
| :--------- | :----- | :---- |
| `---`      | 000    | 0     |
| `--x`      | 001    | 1     |
| `-w-`      | 010    | 2     |
| `-wx`      | 011    | 3     |
| `r--`      | 100    | 4     |
| `r-x`      | 101    | 5     |
| `rw-`      | 110    | 6     |
| `rwx`      | 111    | 7     |

**Print permissions of a file in octal or symbolic notation:**

```bash
[jelle@ubuntu:~]$ stat -c '%A %a' /etc/passwd
-rw-r--r-- 644
```
