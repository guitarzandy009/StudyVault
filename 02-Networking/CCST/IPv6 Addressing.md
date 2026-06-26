
# IPv6 Addressing

## Overview
IPv6 (Internet Protocol version 6) is the successor to IPv4,
developed to solve the exhaustion of IPv4 addresses.
IPv6 uses 128-bit addresses providing 340 undecillion addresses.

## Why IPv6?
- IPv4 exhaustion (only ~4.3 billion addresses)
- No need for NAT — every device can have a public address
- Built-in IPsec security
- Simplified header for faster routing
- Better support for mobile devices and IoT

## Address Format
- 128-bit address
- Written as 8 groups of 4 hexadecimal digits
- Groups separated by colons
- Example: 2001:0db8:85a3:0000:0000:8a2e:0370:7334

## Address Simplification Rules

### Rule 1 — Omit Leading Zeros
- 2001:0db8:0000:0000 → 2001:db8:0:0

### Rule 2 — Double Colon (::)
- Replace one or more consecutive groups of all zeros with ::
- Can only be used ONCE in an address
- 2001:db8:0:0:0:0:0:1 → 2001:db8::1

### Examples
| Full Address                          | Shortened          |
|---------------------------------------|--------------------|
| 2001:0db8:0000:0000:0000:0000:0000:0001 | 2001:db8::1      |
| fe80:0000:0000:0000:0202:b3ff:fe1e:8329 | fe80::202:b3ff:fe1e:8329 |
| 0000:0000:0000:0000:0000:0000:0000:0001 | ::1              |

## IPv6 Address Types

### Unicast
- One to one communication
- **Global Unicast** — Public routable addresses (2000::/3)
- **Link-Local** — Auto-configured, not routable (fe80::/10)
- **Loopback** — ::1 (equivalent to 127.0.0.1)
- **Unspecified** — :: (equivalent to 0.0.0.0)

### Multicast
- One to many (specific group)
- Prefix: ff00::/8
- Replaces IPv4 broadcast

### Anycast
- One to nearest — packet delivered to closest device
- Used for load balancing and redundancy

## IPv6 vs IPv4

| Feature           | IPv4                  | IPv6                        |
|-------------------|-----------------------|-----------------------------|
| Address length    | 32 bits               | 128 bits                    |
| Address format    | Dotted decimal        | Hexadecimal with colons     |
| Total addresses   | ~4.3 billion          | 340 undecillion             |
| NAT required      | Yes                   | No                          |
| Broadcast         | Yes                   | No (uses multicast)         |
| IPsec             | Optional              | Built-in                    |
| Header complexity | Complex               | Simplified                  |
| Configuration     | Manual or DHCP        | SLAAC, DHCPv6, or manual    |

## IPv6 Header
Simplified compared to IPv4 — fixed 40 byte header:
- **Version** — Always 6
- **Traffic Class** — QoS priority
- **Flow Label** — Identifies packet flow
- **Payload Length** — Size of data
- **Next Header** — Identifies next protocol (like TCP, UDP)
- **Hop Limit** — Replaces IPv4 TTL
- **Source Address** — 128-bit source
- **Destination Address** — 128-bit destination

## Address Configuration Methods

### SLAAC — Stateless Address Autoconfiguration
- Device automatically generates its own IPv6 address
- Uses the network prefix from the router + EUI-64 interface ID
- No DHCP server required

### DHCPv6 — Stateful
- DHCPv6 server assigns addresses similar to IPv4 DHCP
- Provides additional info like DNS servers

### Manual
- Static IPv6 address assigned by administrator

## EUI-64
Method to auto-generate the 64-bit interface ID from a MAC address:
1. Take 48-bit MAC address
2. Split in half and insert FF:FE in the middle
3. Flip the 7th bit of the first byte
- Example MAC: 00:1A:2B:3C:4D:5E
- EUI-64: 02:1A:2B:FF:FE:3C:4D:5E

## Special IPv6 Addresses

| Address  | Description                        |
|----------|------------------------------------|
| ::1      | Loopback                           |
| ::       | Unspecified                        |
| fe80::/10 | Link-local (auto-configured)      |
| ff02::1  | All nodes multicast                |
| ff02::2  | All routers multicast              |
| 2000::/3 | Global unicast (public)            |
| fc00::/7 | Unique local (private, like RFC1918)|

## Cisco CLI — IPv6 Commands
```plaintext
ipv6 unicast-routing              ! Enable IPv6 routing
interface GigabitEthernet0/0
 ipv6 address 2001:db8::1/64     ! Assign static IPv6 address
 ipv6 address fe80::1 link-local ! Assign link-local address
 no shutdown
show ipv6 interface brief         ! Check IPv6 interface status
show ipv6 route                   ! View IPv6 routing table
ping ipv6 2001:db8::1             ! Ping IPv6 address
```

## Key Exam Points
- IPv6 is 128 bits — IPv4 is 32 bits
- :: can only appear ONCE in an address
- ::1 is loopback — equivalent to 127.0.0.1
- fe80::/10 is link-local — not routable beyond local segment
- IPv6 has no broadcast — uses multicast instead
- SLAAC allows devices to self-configure without DHCP
- NAT is not needed with IPv6
- Hop Limit replaces IPv4 TTL
- Global unicast addresses start with 2000::/3

## Related Notes
- [[IPv4 Addressing]]
- [[Subnetting]]
- [[DHCP]]
- [[Routing Protocols]]
- [[CCST MOC]]