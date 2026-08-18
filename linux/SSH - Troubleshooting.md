# SSH & Network Troubleshooting

## SSH Configuration & Key Management

### Connection Commands

| Command                                         | Description                                                |
| :---------------------------------------------- | :--------------------------------------------------------- |
| `ssh user@host`                                 | Connect to a remote server                                 |
| `ssh user@host command`                         | Execute a single command on the remote host and exit       |
| `ssh -X user@host`                              | Enable X11 forwarding for graphical applications           |
| `ssh -v user@host`                              | Verbose mode (use `-vv` or `-vvv` for deeper debug output) |
| `ssh -V`                                        | Display SSH client version                                 |
| `exit` / `Ctrl+D`                               | Terminate the current SSH session                          |
| `ssh -o HostKeyAlgorithms=+ssh-rsa user@device` | Explicitly allow legacy `ssh-rsa` key algorithms           |

### Key Management & Agent

| Command                                               | Description                                                      |
| :---------------------------------------------------- | :--------------------------------------------------------------- |
| `ssh-keygen -t ed25519` / `ssh-keygen -t rsa -b 4096` | Generate a new SSH key pair (private & public key)               |
| `ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host`      | Automatically copy public key to remote host's `authorized_keys` |
| `eval $(ssh-agent)`                                   | Start the `ssh-agent` in the current shell context               |
| `ssh-add ~/.ssh/id_ed25519`                           | Add a private key to `ssh-agent`                                 |
| `ssh-add -L`                                          | List public key fingerprints currently loaded in `ssh-agent`     |
| `ssh-add -D`                                          | Delete all keys from `ssh-agent`                                 |
| `scp source destination`                              | Securely copy files over SSH protocol                            |

### Files & Required Permissions

| Path                           | Description                                  | Recommended Permissions              |
| :----------------------------- | :------------------------------------------- | :----------------------------------- |
| `~/.ssh/`                      | User SSH directory                           | `700` (`drwx------`)                 |
| `~/.ssh/id_ed25519` / `id_rsa` | Private key (NEVER share)                    | `600` (`-rw-------`)                 |
| `~/.ssh/id_ed25519.pub`        | Public key                                   | `644` (`-rw-r--r--`)                 |
| `~/.ssh/authorized_keys`       | Public keys allowed to log into this account | `600` (`-rw-------`)                 |
| `~/.ssh/known_hosts`           | Stored fingerprints of remote hosts          | `644` (`-rw-r--r--`)                 |
| `~/.ssh/config`                | Per-user SSH client configuration file       | `600` (`-rw-------`)                 |
| `/etc/ssh/ssh_config`          | Global client configuration file             | Owned by `root`                      |
| `/etc/ssh/sshd_config`         | SSH daemon (server) configuration file       | Owned by `root`                      |
| `/etc/ssh/ssh_host_*`          | Server host keys                             | Private: `600` (root), Public: `644` |

---

## Network Troubleshooting Cheatsheet

### Interface & Link Status

| Command                                                               | Description                                                         |
| :-------------------------------------------------------------------- | :------------------------------------------------------------------ |
| `ip -br l`                                                            | Displays network interfaces in a compact ("brief") overview         |
| `ip addr show <interface>`                                            | Displays detailed IP address configuration for a specific interface |
| `nmcli device status`                                                 | Overview of NetworkManager devices and their operational status     |
| `nmcli -f IP4 connection show '<connection>'`                         | Displays IPv4 settings of a NetworkManager connection               |
| `nmcli connection modify '<connection>' ipv4.addresses <ip>/<prefix>` | Modifies the static IPv4 address of a connection                    |
| `nmcli connection down '<connection>'`                                | Deactivates a network connection                                    |
| `nmcli connection up '<connection>'`                                  | Activates a network connection                                      |

### Routing & DNS

| Command                           | Description                                                        |
| :-------------------------------- | :----------------------------------------------------------------- |
| `ip route`                        | Displays the IP routing table                                      |
| `cat /etc/resolv.conf`            | Displays currently configured DNS nameservers                      |
| `resolvectl dns`                  | Displays per-interface DNS configuration (`systemd-resolved`)      |
| `ping -c1 <ip>`                   | Sends a single ICMP echo packet to test basic layer 3 connectivity |
| `dig @<dns-server> <name> +short` | Performs a quick DNS lookup and outputs a concise response         |
| `nslookup <name> <dns-server>`    | Queries DNS server using the `nslookup` utility                    |

### Services & Sockets

| Command                                    | Description                                                    |
| :----------------------------------------- | :------------------------------------------------------------- |
| `systemctl status <service>`               | Displays operational status and recent log output of a service |
| `systemctl is-active <service>`            | Checks if a service is currently active (`running`)            |
| `sudo systemctl start <service>`           | Starts a service immediately                                   |
| `sudo systemctl enable --now <service>`    | Enables a service at boot and starts it immediately            |
| `sudo ss -tulpn \| grep <service-or-port>` | Displays listening TCP/UDP sockets and associated processes    |

### Firewall (`firewalld`)

| Command                                               | Description                                                    |
| :---------------------------------------------------- | :------------------------------------------------------------- |
| `sudo firewall-cmd --list-all`                        | Displays active firewall zone, interfaces, services, and ports |
| `sudo firewall-cmd --add-service=http --permanent`    | Permanently allows HTTP service through the firewall           |
| `sudo firewall-cmd --add-port=<port>/tcp --permanent` | Permanently opens a specific TCP port                          |
| `sudo firewall-cmd --reload`                          | Reloads firewall configuration to apply permanent changes      |

### Logging & Live Monitoring

| Command                              | Description                                        |
| :----------------------------------- | :------------------------------------------------- |
| `sudo journalctl -fl`                | Streams `systemd` journal logs in real-time        |
| `sudo journalctl -u <service> -f -l` | Streams real-time logs for a specific service unit |
| `tail -f /var/log/<logfile>`         | Monitors classic log files in real-time            |

### Configuration Verification

| Command                                  | Description                                          |
| :--------------------------------------- | :--------------------------------------------------- |
| `sudo apachectl configtest`              | Validates Apache HTTP Server configuration syntax    |
| `sudo nginx -t`                          | Validates Nginx web server configuration syntax      |
| `sudo named-checkconf`                   | Validates BIND DNS `named.conf` configuration syntax |
| `sudo named-checkzone <zone> <zonefile>` | Validates syntax of a BIND DNS zone file             |
| `sudo vsftpd -t`                         | Tests VSFTPD daemon configuration                    |

### Client Testing & Connectivity Tools

| Command                               | Description                                                  |
| :------------------------------------ | :----------------------------------------------------------- |
| `curl -v http://<ip>[:<port>]/<path>` | HTTP request test with verbose headers and handshake details |
| `dig <name> @<dns-server>`            | Standard DNS lookup query                                    |
| `nc -vz <ip> <port>`                  | Checks if a remote TCP port is reachable (Netcat)            |
| `nmap -p <port-range> <ip>`           | Performs a port scan on a target host                        |
