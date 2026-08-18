# Cisco CCNA Configuration & Concepts Cheat Sheet

**Table of contents**

- [1. Basic Device Configuration](#1-basic-device-configuration)
  - [Hostname, Passwords & Encryption](#hostname-passwords--encryption)
  - [SSH Configuration (Required Settings)](#ssh-configuration-required-settings)
  - [Remote Management (SVI) & Default Gateway](#remote-management-svi--default-gateway)
  - [Interface & IP Configuration](#interface--ip-configuration)
  - [Save & Verify Configuration](#save--verify-configuration)
- [2. Switching Concepts & Security](#2-switching-concepts--security)
  - [MAC Address Table Management](#mac-address-table-management)
  - [Port Security Configuration](#port-security-configuration)
- [3. VLANs & Trunking](#3-vlans--trunkingvlan-management)
  - [VLAN Management](#vlan-management)
  - [Access & Voice Ports](#access--voice-ports)
  - [Trunk Ports](#trunk-ports)
  - [Verification]()
- [4. Inter-VLAN Routing](#4-inter-vlan-routing)
  - [Router-on-a-Stick (ROAS)](#router-on-a-stick-roas)
  - [Layer 3 Switch (SVI & Routed Ports)](#layer-3-switch-svi--routed-ports)
- [5. Spanning Tree Protocol (STP)](#5-spanning-tree-protocol-stp)
  - [Root Bridge & Port Tuning](#root-bridge--port-tuning)
  - [PortFast & BPDU Guard](#portfast--bpdu-guard)
  - [Verification & Troubleshooting](#verification--troubleshooting)
- [6. EtherChannel (LAG)](#6-etherchannel-lag)
  - [Static / Manual (ON)](#static--manual-on-)
  - [PAgP (Cisco Proprietary)](#pagp-cisco-proprietary)
  - [LACP (IEEE 802.3ad)](#lacp-ieee-8023ad)
  - [Verification]()
- [7. DHCPv4 Configuration](#7-dhcpv4-configuration)
  - [DHCP Server Configuration](#dhcp-server-configuration)
  - [DHCP Relay Agent](#dhcp-relay-agent)
  - [Router as DHCP Client](#router-as-dhcp-client)
- [8. IPv6 Addressing, SLAAC & DHCPv6](#8-ipv6-addressing-slaac--dhcpv6)
  - [RA Flags Matrix](#ra-flags-matrix)
  - [SLAAC Only (1-0-0)](#slaac-only-1-0-0)
  - [Stateless DHCPv6 / SLAAC + DHCP (1-1-0)](#stateless-dhcpv6--slaac--dhcp-1-1-0)
  - [Stateful DHCPv6 / DHCP Only (0-0-1)](#stateful-dhcpv6--dhcp-only-0-0-1)
- [9. First Hop Redundancy Protocol (HSRP)](#9-first-hop-redundancy-protocol-hsrp)
  - [HSRP Configuration](#hsrp-configuration)
- [10. Routing Concepts & Static Routing](#10-routing-concepts--static-routing)
  - [Routing Table Codes](#routing-table-codes)
  - [Static Route Types](#static-route-types)
  - [Verification]()

## 1. Basic Device Configuration

### Hostname, Passwords & Encryption

```
configure terminal
hostname <device-name>
enable secret <password>
service password-encryption
banner motd # Authorized Access Only! #

line console 0
 password <password>
 login
 exit

line vty 0 15
 password <password>
 login local
 transport input ssh
 exit
```

### SSH Configuration (Required Settings)

```
ip domain-name example.com
crypto key generate rsa general-keys modulus 2048
ip ssh version 2
username <username> secret <password>
```

### Remote Management (SVI) & Default Gateway

```
interface vlan <management-vlan-id>
 ip address <ip-address> <subnet-mask>
 no shutdown
 exit

ip default-gateway <gateway-ip>
```

### Interface & IP Configuration

```
interface <interface-id>
 description <description-text>
 ip address <ip-address> <subnet-mask>
 no shutdown
 exit
```

### Save & Verify Configuration

```
copy running-config startup-config
show running-config
show startup-config
show ip interface brief
```

## 2. Switching Concepts & Security

### MAC Address Table Management

```
show mac address-table
show mac address-table dynamic
clear mac address-table dynamic
```

### Port Security Configuration

```
interface <interface-id>
 switchport mode access
 switchport port-security
 switchport port-security maximum <1-132>
 switchport port-security mac-address sticky
 switchport port-security violation {protect | restrict | shutdown}
 exit
```

### Verification & Maintenance

```
show port-security
show port-security interface <interface-id>
clear port-security sticky
```

## 3. VLANs & Trunking

### VLAN Management

```
vlan <vlan-id>
 name <vlan-name>
 exit

no vlan <vlan-id>
```

### Access & Voice Ports

```
interface <interface-id>
 switchport mode access
 switchport access vlan <vlan-id>
 mls qos trust cos
 switchport voice vlan <voice-vlan-id>
 exit
```

### Trunk Ports

```
interface <interface-id>
 switchport mode trunk
 switchport trunk native vlan <vlan-id>
 switchport trunk allowed vlan <vlan-list>
 exit
```

### Verification

```
show vlan brief
show interfaces trunk
show interfaces <interface-id> switchport
```

## 4. Inter-VLAN Routing

### Router-on-a-Stick (ROAS)

#### Router Configuration

```
interface <physical-interface>
 no shutdown
 exit

interface <physical-interface>.<subinterface-number>
 encapsulation dot1Q <vlan-id> [native]
 ip address <ip-address> <subnet-mask>
 exit
```

#### Switch Trunk Configuration

```
interface <interface-id>
 switchport mode trunk
 switchport trunk allowed vlan <vlan-list>
 exit
```

### Layer 3 Switch (SVI & Routed Ports)

#### 1. Enable Routing & SVIsC

```
ip routing

vlan 10
 name Sales
 exit

interface vlan 10
 ip address 192.168.10.1 255.255.255.0
 no shutdown
 exit
```

#### 2. Routed Interface (Router Port on Switch)

```
interface <interface-id>
 no switchport
 ip address <ip-address> <subnet-mask>
 no shutdown
 exit
```

## 5. Spanning Tree Protocol (STP)

### Root Bridge & Port Tuning

```
! Set Root Bridge
spanning-tree vlan <vlan-id> root primary
! OR set priority manually (increments of 4096)
spanning-tree vlan <vlan-id> priority <priority-value>

! Change Port Cost
interface <interface-id>
 spanning-tree vlan <vlan-id> cost <cost-value>
 exit
```

### PortFast & BPDU Guard

```
interface <interface-id>
 switchport mode access
 spanning-tree portfast
 spanning-tree bpduguard enable
 exit

! Global Enablers
spanning-tree portfast default
spanning-tree portfast bpduguard default
```

### Verification & Troubleshooting

```
show spanning-tree
show spanning-tree vlan <vlan-id>
show spanning-tree interface <interface-id>
show spanning-tree root
```

## 6. EtherChannel (LAG)

### Static / Manual (ON)-

```
interface range <interface-range>
 switchport mode trunk
 channel-group <group-id> mode on
 exit

interface port-channel <group-id>
 switchport mode trunk
 switchport trunk allowed vlan <vlan-list>
 exit
```

### PAgP (Cisco Proprietary)

**Modes:** desirable (initiates), auto (passive).

```
interface range <interface-range>
 switchport mode trunk
 channel-group <group-id> mode {desirable | auto}
 exit
```

### LACP (IEEE 802.3ad)

**Modes:** active (initiates), passive (passive).

```
interface range <interface-range>
 switchport mode trunk
 channel-group <group-id> mode {active | passive}
 exit
```

### VerificationC

```
show etherchannel summary
show etherchannel port-channel
show interfaces port-channel <group-id>
```

## 7. DHCPv4 Configuration

### DHCP Server Configuration

```
ip dhcp excluded-address 192.168.10.1 192.168.10.10

ip dhcp pool <POOL-NAME>
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 8.8.8.8 1.1.1.1
 domain-name example.com
 lease <days> [hours] [minutes]
 exit
```

### DHCP Relay Agent

Configure on the router interface receiving client broadcasts:

```
interface <interface-id>
 ip helper-address <dhcp-server-ip>
 exit
```

### Router as DHCP Client

```
interface <interface-id>
 ip address dhcp
 no shutdown
 exit
```

## 8. IPv6 Addressing, SLAAC & DHCPv6

### RA Flags Matrix

| Mode             | A-Flag (Autoconfig) | O-Flag (Other Config) | M-Flag (Managed Config) | Description                                   |
| :--------------- | :------------------ | :-------------------- | :---------------------- | --------------------------------------------- |
| SLAAC Only       | 1                   | 0                     | 0                       | IP via Prefix + EUI-64/Random, Gateway via RA |
| Stateless DHCPv6 | 1                   | 1                     | 0                       | IP via SLAAC, DNS/Domain via DHCPv6 Server    |
| Stateful DHCPv6  | 0                   | 0                     | 1                       | IP & DNS/Domain fully managed by DHCPv6       |

### SLAAC Only (1-0-0)

```
ipv6 unicast-routing

interface <interface-id>
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1 link-local
 no ipv6 nd managed-config-flag
 no ipv6 nd other-config-flag
 no shutdown
 exit
```

### Stateless DHCPv6 / SLAAC + DHCP (1-1-0)

### Server / Router ConfigurationC

```
ipv6 unicast-routing

ipv6 dhcp pool <POOL-NAME>
 dns-server 2001:db8:acad::53
 domain-name example.com
 exit

interface <interface-id>
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1 link-local
 ipv6 nd other-config-flag
 ipv6 dhcp server <POOL-NAME>
 no shutdown
 exit
```

### Stateful DHCPv6 / DHCP Only (0-0-1)

#### Server / Router Configuration

```
ipv6 unicast-routing

ipv6 dhcp pool <POOL-NAME>
 address prefix 2001:db8:acad:1::/64
 dns-server 2001:db8:acad::53
 domain-name example.com
 exit

interface <interface-id>
 ipv6 address 2001:db8:acad:1::1/64
 ipv6 address fe80::1 link-local
 ipv6 nd managed-config-flag
 ipv6 nd prefix 2001:db8:acad:1::/64 no-autoconfig
 ipv6 dhcp server <POOL-NAME>
 no shutdown
 exit
```

### Client Interface

```
interface <interface-id>
 ipv6 enable
 ipv6 address dhcp
 no shutdown
 exit
```

## 9. First Hop Redundancy Protocol (HSRP)

### HSRP Configuration

```
interface <interface-id>
 ip address <real-ip> <subnet-mask>
 standby version 2
 standby <group-number> ip <virtual-ip>
 standby <group-number> priority <0-255>
 standby <group-number> preempt
 exit
```

### Verification

```
show standby
show standby brief
```

## 10. Routing Concepts & Static Routing

### Routing Table Codes

- `L` - Local Interface Address (/32 or /128)
- `C` - Directly Connected Network
- `S` - Static Route
- `O` - OSPF Dynamic Route
- `D` - EIGRP Dynamic Route
- `*` - Candidate Default Route

### Static Route Types

#### Next-Hop Static Route

```
ip route <network> <mask> <next-hop-ip>
! Example:
ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

#### Directly Connected Static Route

```
ip route <network> <mask> <exit-interface>
! Example:
ip route 192.168.2.0 255.255.255.0 GigabitEthernet0/0/0
```

#### Fully Specified Static Route

```
ip route <network> <mask> <exit-interface> <next-hop-ip>
! Required for IPv6 with link-local next-hop:
ipv6 route 2001:db8:acad:2::/64 GigabitEthernet0/0/0 fe80::2
```

#### Default Static Route

```
! IPv4 Default Route
ip route 0.0.0.0 0.0.0.0 <next-hop-ip | exit-interface>

! IPv6 Default Route
ipv6 route ::/0 <next-hop-ip | exit-interface>
```

#### Floating Static Route (Backup Path)

```
! Main Route (AD = 1)
ip route 0.0.0.0 0.0.0.0 10.0.0.1

! Backup Route (AD = 5)
ip route 0.0.0.0 0.0.0.0 10.0.0.5 5
```

#### Verification

```
show ip route
show ip route static
show ipv6 route
show ip protocols
show running-config | section ip route
```
