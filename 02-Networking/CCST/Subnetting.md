
# Subnetting

## Overview
Subnetting is the process of dividing a network into smaller
sub-networks (subnets). It improves network performance,
security, and efficient use of IP address space.

## Why Subnet?
- Reduces network congestion by limiting broadcast domains
- Improves security by isolating network segments
- Efficient use of limited IPv4 address space
- Easier network management and troubleshooting

## Key Terms
- **Network Address** — First address in a subnet, identifies the subnet itself
- **Broadcast Address** — Last address in a subnet, sent to all hosts
- **Usable Hosts** — All addresses between network and broadcast
- **Subnet Mask** — Defines network vs host portion
- **CIDR** — Classless Inter-Domain Routing notation (e.g. /24)

## Subnetting Cheat Sheet

| CIDR | Subnet Mask     | Subnets* | Hosts  | Increment |
|------|-----------------|----------|--------|-----------|
| /24  | 255.255.255.0   | 1        | 254    | 256       |
| /25  | 255.255.255.128 | 2        | 126    | 128       |
| /26  | 255.255.255.192 | 4        | 62     | 64        |
| /27  | 255.255.255.224 | 8        | 30     | 32        |
| /28  | 255.255.255.240 | 16       | 14     | 16        |
| /29  | 255.255.255.248 | 32       | 6      | 8         |
| /30  | 255.255.255.252 | 64       | 2      | 4         |

*Subnets relative to a /24 base network

## The Subnetting Formula
- **Subnets** = 2^(borrowed bits)
- **Hosts per subnet** = 2^(host bits) - 2

## Binary Basics for Subnetting

| Bit Position | 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|--------------|-----|----|----|----|---|---|---|---|
| Binary       | 1   | 1  | 1  | 1  | 1 | 1 | 1 | 1 |

- Full octet (255) = 11111111
- 128 = 10000000
- 192 = 11000000
- 224 = 11100000
- 240 = 11110000
- 248 = 11111000
- 252 = 11111100

## Step-by-Step Subnetting Method

### Given: 192.168.1.0/26 — How many subnets and hosts?

**Step 1 — Identify the class and default mask**
- 192.168.1.0 is Class C → default mask /24

**Step 2 — Find borrowed bits**
- /26 means 26 bits for network, 6 bits for hosts
- Borrowed bits = 26 - 24 = 2

**Step 3 — Calculate subnets**
- Subnets = 2^2 = 4

**Step 4 — Calculate hosts**
- Host bits = 32 - 26 = 6
- Hosts = 2^6 - 2 = 62

**Step 5 — Find the increment**
- Increment = 256 - 192 = 64

**Step 6 — List the subnets**

| Subnet | Network Address | Usable Range            | Broadcast     |
|--------|-----------------|-------------------------|---------------|
| 1      | 192.168.1.0     | 192.168.1.1 - .62       | 192.168.1.63  |
| 2      | 192.168.1.64    | 192.168.1.65 - .126     | 192.168.1.127 |
| 3      | 192.168.1.128   | 192.168.1.129 - .190    | 192.168.1.191 |
| 4      | 192.168.1.192   | 192.168.1.193 - .254    | 192.168.1.255 |

## VLSM — Variable Length Subnet Masking
- Allows different subnets to have different sizes
- More efficient use of address space
- Used in real-world networks and OSPF routing

### VLSM Example
Given 192.168.1.0/24, allocate for:
- Office A: 50 hosts → needs /26 (62 hosts)
- Office B: 25 hosts → needs /27 (30 hosts)
- Office C: 10 hosts → needs /28 (14 hosts)
- WAN Link: 2 hosts → needs /30 (2 hosts)

## Magic Number Method (Quick Subnetting)
1. Find the interesting octet (where subnetting occurs)
2. Magic number = 256 - subnet mask octet value
3. Count up in increments of the magic number

### Example: /28 = 255.255.255.240
- Magic number = 256 - 240 = 16
- Subnets: .0, .16, .32, .48, .64 ... .240

## Practice Problems

### Problem 1
**Network:** 172.16.0.0/20
- Subnet mask?
- Number of hosts?
- Network address?
- Broadcast address?

### Problem 2
**Network:** 10.0.0.0/8 subnetted to /16
- How many subnets?
- How many hosts per subnet?

### Problem 3
**Host:** 192.168.10.130/27
- Network address?
- Broadcast address?
- Usable host range?

## Key Exam Points
- Network address = first address, not usable
- Broadcast address = last address, not usable
- Usable hosts = 2^(host bits) - 2
- /30 gives only 2 usable hosts — used for point-to-point WAN links
- /32 = single host route (no subnetting)
- Magic number = 256 - subnet mask value in interesting octet
- VLSM allows mixed subnet sizes on the same network

## Related Notes
- [[IPv4 Addressing]]
- [[IPv6 Addressing]]
- [[VLSM]]
- [[CIDR]]
- [[CCST MOC]]