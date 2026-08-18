# Troubleshooting a Network Service

This guide follows the bottom-up approach of the TCP/IP stack (Layer 1 to Layer 4).

## TL;DR Checklist

### 1. Link Layer

- Check physical connections (cables and port LEDs)
- Check interface status: `ip link`

### 2. Network Layer

- Check IP address: `ip a`
- Check routing & gateway connectivity: `ip r` + `ping <default_gateway>`
- Check DNS configuration: `cat /etc/resolv.conf` + `dig www.google.com @<DNS_IP> +short`

### 3. Transport Layer

- Check service status:

  ```bash
  sudo systemctl status SERVICE.service
  sudo systemctl start --now SERVICE.service
  sudo systemctl enable SERVICE.service
  ```

- Check listening ports: sudo ss -tlnp

- Check firewall settings:

  ```bash
  sudo firewall-cmd --list-all
  sudo firewall-cmd --add-service=SERVICE --permanent
  sudo firewall-cmd --add-port=PORT/tcp --permanent
  sudo systemctl restart firewalld
  ```

### 4. Application Layer

- Monitor real-time logs: `sudo journalctl -f -u SERVICE.service`
- Validate configuration and restart: `sudo systemctl restart SERVICE.service`

## General Guidelines

- Use a checklist or automation tools (scripts, Ansible, Serverspec, etc.).
- Keep documentation up-to-date.
- Be thorough and do not skip basic diagnostic steps.
- Use error messages as guidance for troubleshooting.
- Work in small steps and verify each change incrementally.
- Test assumptions continuously.
- Always maintain backups of configuration files before modifying them.
- Always validate configuration syntax prior to restarting services.
- Know which log files are relevant for the service.
- Keep a second terminal open for real-time logging (`journalctl -f`).

## Bottom-Up Approach

Follow this precise sequence for every network service incident:

1. **Link Layer:** Physical cabling, interface state, carrier signal.
2. **Network Layer:** IP address assignment, default routing, DNS resolution.
3. **Transport Layer:** Service status, socket bindings, local firewall rules.
4. **Application Layer:** Service configuration files, application logs, end-to-end availability.

### 1. Link Layer

Verify physical network connectivity:

- Is the target system powered on?
- Is the network cable properly attached?
  - _VirtualBox_: Settings -> Network -> Advanced -> Verify Cable Connected is checked.
- Are the link LEDs active on both the NIC and switch port?
  - Test with a replacement cable if necessary.
  - On managed switches, LED colors indicate link speed (e.g., 100 Mbps vs. 1 Gbps).
- Verify state using `ip link`:
  - `UP`: Interface is administratively and operationally active.
  - `NO-CARRIER`: No physical signal/link detected.

### 2. Network Layer

Proper network communication requires a valid IP address, a default gateway, and at least one DNS server.

#### IP Address Configuration

Interface configuration files are located in `/etc/sysconfig/network-scripts/ifcfg-IFACE` (on RHEL/CentOS systems) or managed via NetworkManager.

Check status using:

```bash
ip a
```

**Common Issues:**

- No IP Address assigned: DHCP server is unreachable or exhausted its pool.
- 169.254.x.x (APIPA): Link-local fallback address; indicates failed DHCP negotiation.
- Unexpected IP Subnet: Incorrect static assignment, or forgotten static configuration that should be DHCP.
- "Network unreachable" error: Subnet mask misconfiguration.

#### Default Gateway

Check current routing table:

```bash
ip r
```

Look for the entry starting with `default via <gateway_ip>`.

**Common Issues:**

- Missing default gateway (frequently accompanies IP assignment issues).
- Incorrect gateway IP due to invalid DHCP scope or leftover manual settings.

#### DNS Resolution

Check configured name servers:

```bash
cat /etc/resolv.conf
```

#### Testing LAN Connectivity

Execute the following verification steps in order:

```bash
# 1. Ping the default gateway
ping -c 4 <gateway_ip>

# 2. Ping another host on the local network
ping -c 4 <local_host_ip>

# 3. Test DNS resolution explicitly against a target server
dig [www.google.com](https://www.google.com) @<DNS_IP> +short
```

> **Note:** ICMP traffic (ping) may be blocked by network firewalls or host policies.

### 3. Transport Layer

Verify whether the service is actively running, bound to the expected interface/port, and accessible through the local firewall.

#### Service Status and Port Binding

Check service state:

```bash
sudo systemctl status SERVICE.service
```

Start and enable service on boot:

```bash
sudo systemctl start SERVICE.service
sudo systemctl enable SERVICE.service
```

Inspect active listening sockets:

```bash
sudo ss -tlnp
```

Verify the process is listening on the correct IP address (e.g., `0.0.0.0:PORT` or `127.0.0.1:PORT`) and port number.

#### Local Firewall (firewalld)

Inspect active rules and assigned zone:

```bash
sudo firewall-cmd --list-all
```

Verify that the target interface is in the active zone, and that required services or ports are permitted. Allow traffic as needed:

```bash
# Allow by service name
sudo firewall-cmd --add-service=SERVICE --permanent

# Allow by explicit port/protocol
sudo firewall-cmd --add-port=PORT/tcp --permanent

# Reload firewall configuration
sudo systemctl restart firewalld
```

### 4. Application Layer

#### Log Analysis

Monitor systemd journal stream:

```bash
sudo journalctl -f -u SERVICE.service
```

For services with dedicated log files, check specific application logs (e.g., `/var/log/httpd/error_log` or `/var/log/nginx/error.log`).

#### Configuration Files

Always take a backup copy before making edits (`cp config.conf config.conf.bak`).

Validate configuration syntax using service-specific tools (e.g., `apachectl configtest`, `nginx -t`, or `named-checkconf`).

Restart the service after applying changes:

```bash
sudo systemctl restart SERVICE.service
```

#### Service Availability Testing

Test locally via loopback first, then remotely from another host.

```bash
# Execute a port scan from the remote host
sudo nmap -sS -p PORT HOST

# Test application response using HTTP clients
curl -I http://HOST/
wget http://HOST/
```

## VirtualBox Networking Reference

### Default Interface Naming

| Adapter   | Linux Device Name |
| :-------- | :---------------- |
| Adapter 1 | `enp0s3`          |
| Adapter 2 | `enp0s8`          |
| Adapter 3 | `enp0s9`          |
| Adapter 4 | `enp0s10`         |

### NAT Interface Default Settings

| Parameter       | Default Value  |
| :-------------- | :------------- |
| Subnet          | `10.0.2.0/24`  |
| Assigned IP     | `10.0.2.15/24` |
| Default Gateway | `10.0.2.2`     |
| DNS Server      | `10.0.2.3`     |

> **Note:** The VM can reach external networks and the Internet, but is not directly accessible from the host operating system.

### Host-Only Interface Defaults

Host-only adapters enable isolated communication between the host OS and virtual machines.

| Component                | IP Range / Address                  |
| :----------------------- | :---------------------------------- |
| Host Adapter IP          | `192.168.56.1/24`                   |
| VirtualBox DHCP Server   | `192.168.56.100`                    |
| Dynamic IP Pool Range    | `192.168.56.101` - `192.168.56.254` |
| Recommended Static Range | `192.168.56.2` - `192.168.56.99`    |

> Modifying Host-Only adapter settings may require restarting all running VMs and the main VirtualBox application process.

### VirtualBox Troubleshooting Checklist

1. **Verify Host Configuration:** Ensure the Host-Only network settings (IP range, DHCP status) are consistent in VirtualBox Network Manager.
2. **Verify Host IP:** Confirm the host OS network interface holds the expected IP (`192.168.56.1`).
3. **VM Settings Checklist:**

- Network adapter is Enabled.
- Cable Connected option is checked.
- Attached to the correct Host-Only Adapter instance.

4. **Guest OS IP Verification:**

- DHCP: Assigned within `192.168.56.101-254`.
- Static: Correctly assigned within `192.168.56.2-99`.

5. **Connectivity Test:** Ping bidirectionally between host and VM (verify ICMP is permitted by OS firewalls).
