# Web Server Hardening

## Firewall Management

| Command                                                                 | Usage                                              |
| :---------------------------------------------------------------------- | :------------------------------------------------- |
| `systemctl status firewalld`                                            | Check the status of the firewall service           |
| `sudo systemctl enable --now firewalld`                                 | Start and enable firewalld                         |
| `sudo firewall-cmd --list-all`                                          | Show the complete configuration of the active zone |
| `firewall-cmd --get-zones`                                              | Show all available zones                           |
| `firewall-cmd --get-active-zones`                                       | Show active zones and their bound interfaces       |
| `sudo firewall-cmd --list-services`                                     | Show allowed services                              |
| `sudo firewall-cmd --add-service=http [--permanent]`                    | Allow a specific service                           |
| `sudo firewall-cmd --add-port=8080/tcp [--permanent]`                   | Allow a specific port                              |
| `sudo firewall-cmd --reload`                                            | Reload the firewall configuration                  |
| `sudo firewall-cmd --add-interface=eth0 --zone=public`                  | Bind an interface to a zone                        |
| `sudo firewall-cmd --remove-interface=eth1 --zone=public`               | Remove an interface from a zone                    |
| `sudo firewall-cmd --panic-on`                                          | Enable panic mode (block all network traffic)      |
| `sudo firewall-cmd --panic-off`                                         | Disable panic mode                                 |
| `sudo firewall-cmd --zone=external --change-interface=eth0 --permanent` | Assign an interface to a different zone            |
| `sudo firewall-cmd --permanent --new-policy=internal-external`          | Create a new policy                                |
| `sudo sysctl -p`                                                        | Reload IP forwarding and kernel parameters         |

## SELinux

| Command                                | Usage                                                       | Example                                                                         |
| :------------------------------------- | :---------------------------------------------------------- | :------------------------------------------------------------------------------ |
| `getenforce`                           | Display current SELinux mode                                | `getenforce`                                                                    |
| `setenforce [0 / 1]`                   | Temporarily change mode (0=Permissive, 1=Enforcing)         | `sudo setenforce 1`                                                             |
| `sestatus`                             | Display detailed SELinux status                             | `sestatus`                                                                      |
| `cat /sys/fs/selinux/enforce`          | Check if enforcing mode is active (1=true, 0=false)         | `cat /sys/fs/selinux/enforce`                                                   |
| `ls -Z`                                | Display SELinux security context of files                   | `ls -lZ /var/www/html`                                                          |
| `ps -Z`                                | Display SELinux security context of processes               | `ps -eZ`                                                                        |
| `ss -Z`                                | Display SELinux context of network sockets/processes        | `sudo ss -tlnZ`                                                                 |
| `id -Z`                                | Display SELinux identity of the current user                | `id -Z`                                                                         |
| `chcon`                                | Temporarily change SELinux context of a file/directory      | `sudo chcon -R -t public_content_t /var/www/html/fileshare`                     |
| `restorecon`                           | Restore default SELinux context of a file/directory         | `sudo restorecon -R /var/www/html`                                              |
| `semanage fcontext`                    | Manage default persistent security contexts for paths       | `sudo semanage fcontext -a -t public_content_t '/var/www/html/fileshare(/.*)?'` |
| `semanage port -l`                     | List port labels managed by SELinux                         | `sudo semanage port -l \| grep 80`                                              |
| `semanage user -l`                     | Display SELinux user and role mappings                      | `sudo semanage user -l`                                                         |
| `getfattr`                             | Display extended file attributes (including SELinux labels) | `getfattr -d -n security.selinux file.txt`                                      |
| `getsebool -a`                         | List all SELinux booleans and their current status          | `getsebool -a \| grep httpd`                                                    |
| `getsebool [boolean]`                  | Display state of a specific boolean                         | `getsebool httpd_can_network_connect_db`                                        |
| `setsebool [-P] [boolean] on/off`      | Change boolean state (`-P` makes it persistent)             | `sudo setsebool -P httpd_can_network_connect_db on`                             |
| `semanage boolean -l`                  | List and describe all SELinux booleans                      | `sudo semanage boolean -l \| grep httpd`                                        |
| `ausearch`                             | Search SELinux audit logs                                   | `sudo ausearch -m AVC,USER_AVC -ts today`                                       |
| `grep denied /var/log/audit/audit.log` | Manually search audit log for denied actions                | `sudo grep denied /var/log/audit/audit.log`                                     |
| `sealert -l <ID>`                      | Display detailed analysis of a setroubleshoot event         | `sealert -l ad1f25d6-6473-4af5-945a-2246b64eecf5`                               |
| `journalctl -t setroubleshoot`         | View SELinux troubleshooting messages in systemd journal    | `sudo journalctl -t setroubleshoot`                                             |
