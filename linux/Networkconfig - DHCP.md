# Network Configuration - DHCP

| Command                                 | Usage                                     |
| --------------------------------------- | ----------------------------------------- |
| `ip a`                                  | Show network interfaces and IP addresses  |
| `ip a show dev <interface>`             | Show specific interface                   |
| `ip r`                                  | Show routing table                        |
| `resolvectl dns`                        | Show DNS servers                          |
| `ping -c <n> <host>`                    | Test network connectivity (IPv4)          |
| `ping6 -c <n> <host>`                   | Test network connectivity (IPv6)          |
| `traceroute <host>`                     | Show router path to host                  |
| `tracepath <host>`                      | Simple traceroute without root privileges |
| `ip n`                                  | Show ARP cache                            |
| `ip n get <ip>`                         | Specific ARP entry                        |
| `ip n del <ip> dev <interface>`         | Remove ARP entry                          |
| `ip n flush dev <interface>`            | Flush ARP cache                           |
| `curl ifconfig.me`                      | Show external IP address                  |
| `ip a add/del <ip> dev <interface>`     | Temporary IP change                       |
| `ip route add/del <route>`              | Add/remove routes                         |
| `ifup <interface>`                      | Bring interface up (Debian)               |
| `ifdown <interface>`                    | Bring interface down (Debian)             |
| `nmcli connection show`                 | Show NetworkManager profiles              |
| `nmcli connection up <name>`            | Activate profile                          |
| `nmcli connection modify <name>`        | Modify IP/DNS settings                    |
| `sudo netplan apply`                    | Apply Netplan changes                     |
| `hostnamectl set-hostname <name>`       | Change hostname                           |
| `ip link set <interface> address <mac>` | Change MAC address                        |

## DHCP Server with ISC KEA

| Command                                                          | Usage                                   |
| ---------------------------------------------------------------- | --------------------------------------- |
| `sudo dnf install kea`                                           | Install Kea on EL                       |
| `sudo apt install kea`                                           | Install Kea on Debian/Ubuntu            |
| `systemctl status kea-dhcp4.service`                             | Check DHCPv4 server status              |
| `sudo systemctl enable --now kea-dhcp4`                          | Start and enable DHCPv4 server          |
| `kea-dhcp4 -t /etc/kea/kea-dhcp4.conf`                           | Check configuration for errors          |
| `sudo ss -uln \| grep ':67'`                                     | Check if server is listening on port 67 |
| `journalctl -flu kea-dhcp4`                                      | View real-time DHCP logs                |
| `sudo tcpdump -w kea-dhcp.pcap -v -n -i eth1 port 67 or port 68` | Capture DHCP packets                    |
| `tcpdump -r /vagrant/kea-dhcp.pcap -ne#`                         | View DHCP packets from pcap file        |
