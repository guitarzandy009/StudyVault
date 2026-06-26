
# MAC Addresses & ARP

## Overview
MAC addresses operate at OSI Layer 2 (Data Link) and identify
devices on a local network. ARP (Address Resolution Protocol)
bridges Layer 2 and Layer 3 by mapping IP addresses to MAC addresses.

## MAC Addresses

### What is a MAC Address?
- MAC = Media Access Control
- 48-bit (6 byte) hardware address
- Burned into the NIC (Network Interface Card) by the manufacturer
- Also called: Physical address, Hardware address, BIA (Burned-In Address)
- Operates at OSI Layer 2 (Data Link)

### MAC Address Format
- Written as 6 pairs of hexadecimal digits
- Separators vary by OS:

| Format        | Example                |
|---------------|------------------------|
| Colon         | 00:1A:2B:3C:4D:5E      |
| Hyphen        | 00-1A-2B-3C-4D-5E      |
| Cisco (dots)  | 001a.2b3c.4d5e         |

### MAC Address Structure
| OUI (First 3 bytes)      | Device ID (Last 3 bytes) |
|--------------------------|--------------------------|
| 00:1A:2B                 | 3C:4D:5E                 |
- **OUI** — Organizationally Unique Identifier, assigned to manufacturer
- **Device ID** — Unique identifier assigned by manufacturer

### Finding Your MAC Address
```plaintext
# Linux
ip link show
ifconfig

# Cisco IOS
show interfaces
show mac address-table
```

### Special MAC Addresses
| Address           | Description                        |
|-------------------|------------------------------------|
| FF:FF:FF:FF:FF:FF | Broadcast — sent to all devices    |
| 01:00:5E:xx:xx:xx | IPv4 Multicast                     |
| 33:33:xx:xx:xx:xx | IPv6 Multicast                     |

## ARP — Address Resolution Protocol

### What is ARP?
- Operates at OSI Layer 2/3 boundary
- Resolves IPv4 addresses to MAC addresses
- Essential for communication on local networks
- Defined in RFC 826

### Why ARP is Needed
- IP packets need MAC addresses to be delivered on a LAN
- A device knows the destination IP but needs the MAC address
- ARP discovers the MAC address dynamically

### ARP Process — Step by Step
1. **Host A** wants to send data to 192.168.1.20
2. Host A checks its **ARP cache** — no entry found
3. Host A sends an **ARP Request** (broadcast FF:FF:FF:FF:FF:FF)
   - "Who has 192.168.1.20? Tell 192.168.1.10"
4. All devices on the LAN receive the broadcast
5. **Host B** (192.168.1.20) recognizes its IP and replies
6. Host B sends an **ARP Reply** (unicast back to Host A)
   - "192.168.1.20 is at 00:1A:2B:3C:4D:5E"
7. Host A stores the result in its **ARP cache**
8. Communication proceeds using the MAC address

### ARP Cache
- Temporarily stores IP-to-MAC mappings
- Reduces ARP broadcasts on the network
- Entries expire after a timeout period

```plaintext
# View ARP cache — Linux
arp -a
ip neigh show

# View ARP cache — Cisco IOS
show arp
show ip arp
```

### ARP Cache Example Output
```plaintext
Protocol  Address       Age  Hardware Addr   Type  Interface
Internet  192.168.1.1   0    00a1.2b3c.4d5e  ARPA  Gig0/0
Internet  192.168.1.10  5    00b2.3c4d.5e6f  ARPA  Gig0/0
```

## ARP Message Types

| Type      | Direction | Description                          |
|-----------|-----------|--------------------------------------|
| ARP Request  | Broadcast | "Who has this IP? Tell me your MAC" |
| ARP Reply    | Unicast   | "I have that IP, here is my MAC"    |
| Gratuitous ARP | Broadcast | Device announces its own MAC      |

## Proxy ARP
- Router responds to ARP requests on behalf of devices
- Used when source and destination are on different subnets
- Allows devices to communicate without a default gateway config

## ARP Security Issues

### ARP Spoofing / ARP Poisoning
- Attacker sends fake ARP replies to poison ARP caches
- Associates attacker MAC with legitimate IP address
- Enables Man-in-the-Middle (MitM) attacks
- Can intercept, modify, or drop traffic

### Mitigation
- **Dynamic ARP Inspection (DAI)** — Cisco switch feature
- **Static ARP entries** — Manually set critical entries
- **VPNs & encryption** — Protects data even if intercepted

## IPv6 & ARP
- IPv6 does NOT use ARP
- Replaced by **NDP — Neighbor Discovery Protocol**
- NDP uses ICMPv6 messages instead of broadcasts
- More efficient and secure than ARP

## Layer 2 vs Layer 3 Addressing

| Feature        | MAC Address         | IP Address           |
|----------------|---------------------|----------------------|
| OSI Layer      | Layer 2             | Layer 3              |
| Scope          | Local network only  | Global (routable)    |
| Assigned by    | Manufacturer        | Admin or DHCP        |
| Changes        | Rarely              | Frequently           |
| Format         | 48-bit hex          | 32-bit decimal (v4)  |

## Key Exam Points
- MAC addresses are 48 bits, written in hexadecimal
- First 3 bytes = OUI (manufacturer), last 3 bytes = device ID
- ARP maps IP addresses to MAC addresses
- ARP request is a broadcast, ARP reply is a unicast
- ARP cache stores mappings temporarily to reduce broadcasts
- FF:FF:FF:FF:FF:FF is the broadcast MAC address
- IPv6 uses NDP instead of ARP
- ARP spoofing is a common Layer 2 attack
- Switches use MAC address tables to forward frames

## Related Notes
- [[OSI Model]]
- [[IPv4 Addressing]]
- [[IPv6 Addressing]]
- [[Switching & VLANs]]
- [[Network Security]]
- [[CCST MOC]]