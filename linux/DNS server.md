# DNS server

## Introduction to DNS

| **Command**                      | **Usage**                                               |
| -------------------------------- | ------------------------------------------------------- |
| `getent ahosts <name>`           | Basic DNS lookup without extra tools                    |
| `host <name>`                    | Simple lookup (A, AAAA, MX, reverse)                    |
| `host <name> <dns-server>`       | Query specific DNS server                               |
| `nslookup <name>`                | Classic DNS tool with simple output                     |
| `nslookup <name> <dns-server>`   | Query external DNS server                               |
| `nslookup`                       | Interactive mode                                        |
| `dig <name>`                     | Very comprehensive DNS query, ideal for troubleshooting |
| `dig +short <name>`              | Short output                                            |
| `dig @<server> <name>`           | Query specific name server                              |
| `dig <TYPE> <name>`              | Query specific record types (A/AAAA/MX/NS)              |
| `dig -x <IP>`                    | Reverse lookup                                          |
| `dig ANY <domain>`               | Query all available DNS records                         |
| `dig AXFR <zone> @server`        | Zone transfer (usually blocked)                         |
| `resolvectl dns <link> <server>` | Set DNS server (systemd-resolved)                       |

## The BIND DNS server

| Command                         | Usage                                      |
| ------------------------------- | ------------------------------------------ |
| `systemctl status named`        | Check DNS service                          |
| `systemctl restart named`       | Restart BIND                               |
| `ss -tlnp \| grep named`        | Check listening ports                      |
| `named-checkconf`               | Validate `named.conf`                      |
| `named-checkzone ZONE FILE`     | Validate zone file                         |
| `journalctl -flu named.service` | Live logs                                  |
| `rndc querylog on/off`          | Toggle query logging                       |
| `dig <name>`                    | Perform DNS query                          |
| `dig @server AXFR zone`         | Test zone transfer                         |
| `nslookup`                      | Alternative DNS client (handy SOA display) |

## BIND - Troubleshooting Commands

| Command                         | Usage                  |
| ------------------------------- | ---------------------- |
| `systemctl status named`        | Check status of BIND   |
| `ss -tlnp \| grep named`        | View listening ports   |
| `named-checkconf`               | Validate configuration |
| `named-checkzone <zone> <file>` | Validate zone file     |
| `journalctl -flu named.service` | Follow live logging    |
| `rndc querylog on/off`          | Toggle query logging   |

## Forward lookup zone - example zone file

        ;; Zone file for example.com
        $ORIGIN example.com.
        $TTL 1W

        @ IN SOA ns1.example.com. hostmaster.example.com. (
                24061601  ; Serial
                1D        ; Refresh time
                1H        ; Retry time
                1W        ; Expiry time
                1D )      ; Negative cache TTL

        ; Name servers

            IN  NS     ns1
            IN  NS     ns2

        ; Mail server

            IN  MX     10 srv002

        ; Hosts

        ns1    IN  A      192.0.2.1
        ns2    IN  A      192.0.2.2

        srv001 IN  A      192.0.2.10
        @      IN  A      192.0.2.10
        www    IN  CNAME  srv001

        srv002 IN  A      192.0.2.20
        smtp   IN  CNAME  srv002
        imap   IN  CNAME  srv002

## named.conf -> zone definition forward lookup

    zone "example.com" IN {
        type primary;
        file "example.com";
    };

## Reverse lookup zone - example zone file

        ;; Zone file for reverse lookup zone 2.0.192.in-addr.arpa.
        $ORIGIN 2.0.192.in-addr.arpa.
        $TTL 1W

        @ IN SOA ns1.example.com. hostmaster.example.com. (
                24061601  ; Serial
                1D        ; Refresh time
                1H        ; Retry time
                1W        ; Expiry time
                1D )      ; Negative cache TTL

        ; Name servers

            IN  NS     ns1.example.com.
            IN  NS     ns2.example.com.

        ; Reverse lookup records

        1    IN  PTR    ns1.example.com.
        2    IN  PTR    ns2.example.com.
        10   IN  PTR    srv001.example.com.
        20   IN  PTR    srv002.example.com.

## named.conf -> zone definition reverse lookup

    zone "2.0.192.in-addr.arpa" IN {
        type master;
        file "2.0.192.in-addr.arpa";
    };
