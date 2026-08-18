# tmux Cheat Sheet

**Table of contents**

- [Basic management](#basic-management)
- [List all shortcuts](#list-all-shortcuts)
- [Sessions](#sessions)
- [Windows (tabs)](#windows-tabs)
- [Panes (splits)](#panes-splits)
- [Sync panes](#sync-panes)
- [Resizing panes](#resizing-panes)
- [Copy mode](#copy-mode)
- [Misc](#misc)
- [Configurations options](#configurations-options)
- [Resources](#resources)

## Basic management

| Command                       | Description                 |
| ----------------------------- | --------------------------- |
| `tmux`                        | start new                   |
| `tmux new -s myname`          | start new with session name |
| `tmux a # (or at, or attach)` | attach                      |
| `tmux a -t myname`            | attach to named             |
| `tmux ls`                     | list sessions               |
| `tmux kill-session -t myname` | kill session                |
| `tmux kill-server`            | kill all the tmux sessions  |

In tmux, hit the prefix `ctrl+b` and then:

## List all shortcuts

to see all the shortcuts keys in tmux simply use the `bind-key ?`

## Sessions

| Shortcut   | Description   |
| ---------- | ------------- |
| `:new<CR>` | new session   |
| `s`        | list sessions |
| `$`        | name session  |

## Windows (tabs)

| Shortcut | Description     |
| -------- | --------------- |
| `c`      | create window   |
| `w`      | list windows    |
| `n`      | next window     |
| `p`      | previous window |
| `f`      | find window     |
| `,`      | name window     |
| `&`      | kill window     |

## Panes (splits)

| Shortcut     | Description                                                                |
| ------------ | -------------------------------------------------------------------------- |
| `%`          | vertical split                                                             |
| `"`          | horizontal split                                                           |
| `o`          | swap panes                                                                 |
| `q`          | show pane numbers                                                          |
| `x`          | kill pane                                                                  |
| `⍽`          | space - toggle between layouts                                             |
| `<prefix> !` | break pane into window                                                     |
| `<prefix> q` | show pane numbers, when the numbers show up type the key to goto that pane |
| `<prefix> {` | move the current pane left                                                 |
| `<prefix> }` | move the current pane right                                                |
| `<prefix> z` | toggle pane zoom                                                           |

## Sync panes

You can do this by switching to the appropriate window, typing your Tmux prefix (commonly Ctrl-B or Ctrl-A) and then a colon to bring up a Tmux command line, and typing:

```
:setw synchronize-panes
```

You can optionally add on or off to specify which state you want; otherwise the option is simply toggled. This option is specific to one window, so it won't change the way your other sessions or windows operate. When you're done, toggle it off again by repeating the command. [tip source](http://blog.sanctum.geek.nz/sync-tmux-panes/)

## Resizing panes

You can also resize panes if you don't like the layout defaults. I personally rarely need to do this, though it's handy to know how. Here is the basic syntax to resize panes:

    PREFIX : resize-pane -D (Resizes the current pane down)
    PREFIX : resize-pane -U (Resizes the current pane upward)
    PREFIX : resize-pane -L (Resizes the current pane left)
    PREFIX : resize-pane -R (Resizes the current pane right)
    PREFIX : resize-pane -D 20 (Resizes the current pane down by 20 cells)
    PREFIX : resize-pane -U 20 (Resizes the current pane upward by 20 cells)
    PREFIX : resize-pane -L 20 (Resizes the current pane left by 20 cells)
    PREFIX : resize-pane -R 20 (Resizes the current pane right by 20 cells)
    PREFIX : resize-pane -t 2 -L 20 (Resizes the pane with the id of 2 left by 20 cells)

## Copy mode:

Pressing PREFIX [ places us in Copy mode. We can then use our movement keys to move our cursor around the screen. By default, the arrow keys work. we set our configuration file to use Vim keys for moving between windows and resizing panes so we wouldn't have to take our hands off the home row. tmux has a vi mode for working with the buffer as well. To enable it, add this line to `.tmux.conf`:

```
  setw -g mode-keys vi
```

With this option set, we can use `h`, `j`, `k`, and `l` to move around our buffer.

To get out of Copy mode, we just press the ENTER key. Moving around one character at a time isn't very efficient. Since we enabled vi mode, we can also use some other visible shortcuts to move around the buffer.

For example, we can use `w` to jump to the next word and `b` to jump back one word. And we can use `f`, followed by any character, to jump to that character on the same line, and `F` to jump backwards on the line.

| Function              | vi             | emacs       |
| --------------------- | -------------- | ----------- |
| Back to indentation   | `^`            | `M-m`       |
| Clear selection       | `Escape`       | `C-g`       |
| Copy selection        | `Enter`        | `M-w`       |
| Cursor down           | `j`            | `Down`      |
| Cursor left           | `h`            | `Left`      |
| Cursor right          | `l`            | `Right`     |
| Cursor to bottom line | `L`            |             |
| Cursor to middle line | `M`            | `M-r`       |
| Cursor to top line    | `H`            | `M-R`       |
| Cursor up             | `k`            | `Up`        |
| Delete entire line    | `d`            | `C-u`       |
| Delete to end of line | `D`            | `C-k`       |
| End of line           | `$`            | `C-e`       |
| Goto line             | `:`            | `g`         |
| Half page down        | `C-d`          | `M-Down`    |
| Half page up          | `C-u`          | `M-Up`      |
| Next page             | `C-f`          | `Page down` |
| Next word             | `w`            | `M-f`       |
| Paste buffer          | `p`            | `C-y`       |
| Previous page         | `C-b`          | `Page up`   |
| Previous word         | `b`            | `M-b`       |
| Quit mode             | `q`            | `Escape`    |
| Scroll down           | `C-Down or `J` | `C-Down`    |
| Scroll up             | `C-Up` or `K`  | `C-Up`      |
| Search again          | `n`            | `vn`        |
| Search backward       | `?`            | `C-r`       |
| Search forward        | `/`            | `C-s`       |
| Start of line         | `0`            | `C-a`       |
| Start selection       | `Space`        | `C-Space`   |
| Transpose chars       |                | `C-t`       |

## Misc

| Shortcut | Description    |
| -------- | -------------- |
| `d`      | detach         |
| `t`      | big clock      |
| `?`      | list shortcuts |
| `:`      | prompt         |

## Configurations options:

- Mouse support - set to on if you want to use the mouse
  - `set -g mouse on (or off)`
  - `set -g mouse-resize-pane off`
  - `set -g mouse-select-window off`

- Set the default terminal mode to 256color mode
  - `set -g default-terminal "screen-256color"`

- enable activity alerts
  - `setw -g monitor-activity on`
  - `set -g visual-activity on`

- Center the window list
  - `set -g status-justify centre`

- Maximize and restore a pane
  - `unbind Up bind Up new-window -d -n tmp \; swap-pane -s tmp.1 \; select-window -t tmp`
  - `unbind Down`
  - `bind Down last-window \; swap-pane -s tmp.1 \; kill-window -t tmp`

## Resources:

- [tmux: Productive Mouse-Free Development](http://pragprog.com/book/bhtmux/tmux)
- [How to reorder windows](http://superuser.com/questions/343572/tmux-how-do-i-reorder-my-windows)
