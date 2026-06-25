
# IPv4 Addressing

## Overview
IPv4 (Internet Protocol version 4) is the primary addressing scheme
used to identify devices on a network. Each address is 32 bits long,
written as four octets in dotted decimal notation.

## Address Format
- 32-bit address split into 4 octets
- Each octet = 8 bits (0-255)
- Example: 192.168.1.100

## Address Classes

| Class | Range                       | Default Subnet Mask | Use Case          |
|-------|-----------------------------|---------------------|-------------------|
| A     | 1.0.0.0 - 126.255.255.255   | 255.0.0.0 (/8)      | Large networks    |
| B     | 128.0.0.0 - 191.255.255.255 | 255.255.0.0 (/16)   | Medium networks   |
| C     | 192.0.0.0 - 223.255.255.255 | 255.255.255.0 (/24) | Small networks    |
| D     | 224.0.0.0 - 239.255.255.255 | N/A                 | Multicast         |
| E     | 240.0.0.0 - 255.255.255.255 | N/A                 | Experimental      |

## Private Address Ranges (RFC 1918)
These are not routable on the public internet:

| Class | Private Range                  | Subnet Mask     |
|-------|--------------------------------|-----------------|
| A     | 10.0.0.0 - 10.255.255.255      | 255.0.0.0 /8    |
| B     | 172.16.0.0 - 172.31.255.255    | 255.240.0.0 /12 |
| C     | 192.168.0.0 - 192.168.255.255  | 255.255.0.0 /16 |

## Special Addresses
- **127.0.0.1** — Loopback (localhost), tests local TCP/IP stack
- **0.0.0.0** — Unspecified address, used during DHCP discovery
- **255.255.255.255** — Limited broadcast, sent to all hosts on local network
- **169.254.x.x** — APIPA (Automatic Private IP Addressing), assigned when DHCP fails

## Network vs Host Portion
Every IPv4 address has two parts:
- **Network portion** — Identifies the network
- **Host portion** — Identifies the device on that network
- The subnet mask defines the boundary between the two

## Subnet Mask Basics
- Written in dotted decimal or CIDR notation
- Example: 255.255.255.0 = /24
- Binary 1s = network portion
- Binary 0s = host portion

## CIDR Notation

| CIDR | Subnet Mask     | Hosts per Subnet |
|------|-----------------|------------------|
| /8   | 255.0.0.0       | 16,777,214       |
| /16  | 255.255.0.0     | 65,534           |
| /24  | 255.255.255.0   | 254              |
| /25  | 255.255.255.128 | 126              |
| /26  | 255.255.255.192 | 62               |
| /27  | 255.255.255.224 | 30               |
| /28  | 255.255.255.240 | 14               |
| /29  | 255.255.255.248 | 6                |
| /30  | 255.255.255.252 | 2                |

## Calculating Hosts per Subnet
Formula: **2^(host bits) - 2**
- Subtract 2 for network address and broadcast address
- Example: /24 has 8 host bits → 2^8 - 2 = 254 hosts

## Types of IPv4 Transmission
- **Unicast** — One to one
- **Broadcast** — One to all hosts on subnet
- **Multicast** — One to a group (Class D addresses)

## NAT — Network Address Translation
- Allows private IP addresses to communicate on the public internet
- Router translates private IPs to a single public IP
- Conserves public IPv4 address space

## Key Exam Points
- IPv4 addresses are 32 bits / 4 octets
- 127.0.0.1 is always loopback
- 169.254.x.x means DHCP failed
- Private addresses are not routable on the internet
- NAT allows private addresses to reach the internet
- Hosts per subnet = 2^(host bits) - 2
- /24 = 254 usable hosts, most common in small networks

## Related Notes
- [[Subnetting]]
- [[IPv6 Addressing]]
- [[DHCP]]
- [[NAT]]
- [[TCP-IP Model]]
- [[CCST MOC]]