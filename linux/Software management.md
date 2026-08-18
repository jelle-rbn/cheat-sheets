# Chapter 3 - Software Management

## Language & Keyboard Settings

| Task                                         | Command                               |
| :------------------------------------------- | :------------------------------------ |
| Check current language and locale settings   | `localectl status`                    |
| Temporarily change keyboard layout to AZERTY | `sudo loadkeys be-latin1`             |
| Permanently set console keymap to AZERTY     | `sudo localectl set-keymap be-latin1` |
| Permanently set X11/GUI keymap to AZERTY     | `sudo localectl set-x11-keymap be`    |

## Overview of Package Management Levels

| Level / Paradigm                | Package Manager / Source                                 | Typical Use Case                | Example Command                                     |
| :------------------------------ | :------------------------------------------------------- | :------------------------------ | :-------------------------------------------------- |
| **System-level (RPM/Debian)**   | `dnf`, `apt`                                             | OS-wide software & dependencies | `sudo dnf install <pkg>` / `sudo apt install <pkg>` |
| **Application-level**           | `pip` (Python), `npm` (JS), `gem` (Ruby), `CTAN` (TeX)   | Language-specific libraries     | `pip install <pkg>`                                 |
| **Container-based / Sandboxed** | `flatpak` (Red Hat ecosystem), `snap` (Canonical/Ubuntu) | Isolated desktop/server apps    | `flatpak install <app>` / `sudo snap install <app>` |
| **Source Code Compilation**     | GNU Build System (`configure`, `make`)                   | Custom builds from source       | `./configure && make && sudo make install`          |

### Python Environment Management (`pip` / `venv`)

| Task                           | Command                     |
| :----------------------------- | :-------------------------- |
| List installed Python packages | `pip list`                  |
| Install a Python package       | `pip install <package>`     |
| Uninstall a Python package     | `pip uninstall <package>`   |
| Create a virtual environment   | `python3 -m venv .venv`     |
| Activate virtual environment   | `source .venv/bin/activate` |
| Deactivate virtual environment | `deactivate`                |

---

## Enterprise Linux (EL) Package Management

### RPM (Red Hat Package Manager)

_Low-level tool for managing individual `.rpm` packages without resolving remote dependencies._

| Task                                      | Command                       |
| :---------------------------------------- | :---------------------------- |
| List all installed packages               | `rpm -qa`                     |
| Check if a specific package is installed  | `rpm -q <package>`            |
| List files installed by a package         | `rpm -ql <package>`           |
| Install or upgrade a local `.rpm` package | `sudo rpm -Uvh <package>.rpm` |
| Remove a package                          | `sudo rpm -e <package>`       |

### DNF (Dandified YUM)

_High-level package manager that automatically resolves dependencies and manages repositories._

| Task                                            | Command                                |
| :---------------------------------------------- | :------------------------------------- |
| List all available and installed packages       | `dnf list`                             |
| List only installed packages                    | `dnf list --installed`                 |
| List packages available for installation        | `dnf list --available`                 |
| Search package names and descriptions           | `dnf search <term>`                    |
| Display detailed package information            | `dnf info <package>`                   |
| Install a package and its dependencies          | `sudo dnf install <package>`           |
| Upgrade all installed packages                  | `sudo dnf upgrade`                     |
| Remove a package and unneeded dependencies      | `sudo dnf remove <package>`            |
| Find which package provides a specific file     | `dnf provides <file_path>`             |
| Install a software group (environment group)    | `sudo dnf groupinstall "<group_name>"` |
| List active repositories                        | `dnf repolist`                         |
| Display information about a specific repository | `dnf repoinfo <repo_id>`               |

### Repositories & Configuration

| File / Option             | Purpose                                                            |
| :------------------------ | :----------------------------------------------------------------- |
| `/etc/dnf/dnf.conf`       | Main DNF configuration file                                        |
| `/etc/yum.repos.d/*.repo` | Repository configuration files                                     |
| `--enablerepo=<repo_id>`  | Temporarily enable a specific repository during command execution  |
| `--disablerepo=<repo_id>` | Temporarily disable a specific repository during command execution |

### Cache Management

| Task                                        | Command / Path       |
| :------------------------------------------ | :------------------- |
| Clear all cached metadata and package files | `sudo dnf clean all` |
| Default cache directory location            | `/var/cache/dnf`     |

---

## Debian & Ubuntu Package Management

### dpkg

_Low-level tool for managing individual `.deb` files._

| Task                                              | Command               |
| :------------------------------------------------ | :-------------------- |
| List all installed packages                       | `dpkg -l`             |
| Display status of a specific package              | `dpkg -l <package>`   |
| Find which package owns/installed a specific file | `dpkg -S <file_path>` |
| List all files installed by a package             | `dpkg -L <package>`   |

### APT (Advanced Package Tool)

_High-level package manager (`apt` is recommended for interactive use; `apt-get` / `apt-cache` are preferred for scripts)._

| Task                                           | Modern Interface (`apt`)     | Classic Interface (`apt-get` / `apt-cache`) |
| :--------------------------------------------- | :--------------------------- | :------------------------------------------ |
| Update local package index                     | `sudo apt update`            | `sudo apt-get update`                       |
| Upgrade installed packages                     | `sudo apt upgrade`           | `sudo apt-get upgrade`                      |
| Perform safe/dist upgrade                      | `sudo apt full-upgrade`      | `sudo apt-get dist-upgrade`                 |
| Install package(s)                             | `sudo apt install <package>` | `sudo apt-get install <package>`            |
| Search for packages                            | `apt search <term>`          | `apt-cache search <term>`                   |
| Remove package (keep configuration)            | `sudo apt remove <package>`  | `sudo apt-get remove <package>`             |
| Purge package (remove package + configuration) | `sudo apt purge <package>`   | `sudo apt-get purge <package>`              |
| Remove unused dependency packages              | `sudo apt autoremove`        | `sudo apt-get autoremove`                   |
| Clear local package archive cache              | `sudo apt clean`             | `sudo apt-get clean`                        |
