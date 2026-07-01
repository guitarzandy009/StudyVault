# DNS & DHCP

## Overview
DNS and DHCP are two of the most fundamental network services.
DNS resolves human-readable names to IP addresses. DHCP
automatically assigns IP configuration to network devices.

---

## DNS — Domain Name System

### What is DNS?
- Resolves domain names to IP addresses
- Operates at OSI Layer 7 (Application)
- Uses port 53 (UDP for queries, TCP for zone transfers)
- Distributed hierarchical database

### Why DNS?
- Humans remember names (google.com)
- Computers need IP addresses (142.250.80.46)
- DNS bridges the gap automatically

### DNS Hierarchy
```plaintext
Root (.)
├── Top Level Domains (TLD) — .com .org .net .edu .gov
│   ├── Second Level Domains — google.com, cisco.com
│   │   └── Subdomains — mail.google.com, www.cisco.com
```

### DNS Record Types

| Record | Full Name          | Purpose                              |
|--------|--------------------|--------------------------------------|
| A      | Address            | Maps hostname to IPv4 address        |
| AAAA   | Quad-A             | Maps hostname to IPv6 address        |
| CNAME  | Canonical Name     | Alias for another hostname           |
| MX     | Mail Exchange      | Mail server for a domain             |
| NS     | Name Server        | Authoritative name server for domain |
| PTR    | Pointer            | Reverse DNS — IP to hostname         |
| SOA    | Start of Authority | Zone authority and serial info       |
| TXT    | Text               | SPF, DKIM, domain verification       |

### DNS Resolution Process — Step by Step
1. User types **www.google.com** in browser
2. OS checks **local DNS cache** — if found, done
3. OS checks **hosts file** (/etc/hosts) — if found, done
4. Query sent to **Recursive Resolver** (usually ISP or 8.8.8.8)
5. Resolver checks its cache — if found, returns answer
6. Resolver queries **Root Name Server** — returns TLD server
7. Resolver queries **TLD Name Server** (.com) — returns authoritative NS
8. Resolver queries **Authoritative Name Server** — returns IP address
9. Resolver caches result and returns IP to client
10. Browser connects to IP address

### DNS Query Types
| Type       | Description                                    |
|------------|------------------------------------------------|
| Recursive  | Resolver does all the work, returns final answer |
| Iterative  | Server returns best answer it has, client continues |
| Inverse    | Resolves IP address back to hostname (PTR)     |

### DNS Caching & TTL
- **TTL (Time to Live)** — How long a record is cached (in seconds)
- Reduces DNS query load on servers
- Lower TTL = faster propagation of changes
- Higher TTL = less DNS traffic

### Common DNS Ports
| Protocol | Port | Use                          |
|----------|------|------------------------------|
| UDP 53   | 53   | Standard DNS queries         |
| TCP 53   | 53   | Zone transfers, large queries|
| DoT      | 853  | DNS over TLS (encrypted)     |
| DoH      | 443  | DNS over HTTPS (encrypted)   |

### Public DNS Servers
| Provider    | Primary      | Secondary    |
|-------------|--------------|--------------|
| Google      | 8.8.8.8      | 8.8.4.4      |
| Cloudflare  | 1.1.1.1      | 1.0.0.1      |
| OpenDNS     | 208.67.222.222 | 208.67.220.220 |

### DNS Troubleshooting Commands
```plaintext
# Linux
nslookup google.com             ! Query DNS for a domain
dig google.com                  ! Detailed DNS query
dig google.com MX               ! Query specific record type
dig -x 8.8.8.8                  ! Reverse DNS lookup
cat /etc/resolv.conf            ! View configured DNS servers
cat /etc/hosts                  ! View local hosts file

# Cisco IOS
ip domain-name lab.local        ! Set domain name
ip name-server 8.8.8.8          ! Set DNS server
nslookup google.com             ! DNS lookup from router
```

### DNS Security Issues
- **DNS Spoofing/Poisoning** — Attacker injects false DNS records
- **DNS Hijacking** — Redirects DNS queries to malicious server
- **DNS Amplification** — DDoS attack using DNS as amplifier
- **DNSSEC** — DNS Security Extensions, digitally signs records

---

## DHCP — Dynamic Host Configuration Protocol

### What is DHCP?
- Automatically assigns IP configuration to network devices
- Operates at OSI Layer 7 (Application)
- Uses UDP ports 67 (server) and 68 (client)
- Eliminates need for manual IP configuration

### What DHCP Assigns (The DORA Process)
- IP address
- Subnet mask
- Default gateway
- DNS server addresses
- Lease time

### DORA Process — Step by Step
```plaintext
Client                          Server
  |                               |
  |--- DHCP Discover (broadcast) →|  ! Client broadcasts looking for DHCP server
  |                               |
  |← DHCP Offer (unicast/bcast) --|  ! Server offers IP configuration
  |                               |
  |--- DHCP Request (broadcast) →|   ! Client requests offered configuration
  |                               |
  |← DHCP Acknowledge (unicast) --|  ! Server confirms lease
  |                               |
```

### DORA Explained
| Step        | Direction | Description                             |
| ----------- | --------- | --------------------------------------- |
| Discover    | Broadcast | Client looks for available DHCP servers |
| Offer       | Unicast   | Server offers IP address and config     |
| Request     | Broadcast | Client requests the offered config      |
| Acknowledge | Unicast   | Server confirms and assigns the lease   |

### DHCP Lease
- IP address assigned for a limited time (lease period)
- Client attempts renewal at 50% of lease time
- If renewal fails, tries again at 87.5% of lease time
- If lease expires, client must restart DORA process

### DHCP Configuration (Cisco IOS)
```plaintext
! Configure DHCP pool
Router(config)# ip dhcp pool LAN_POOL
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

! Exclude addresses from pool (routers, servers, printers)
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.20

! Verify DHCP
show ip dhcp binding            ! View assigned leases
show ip dhcp pool               ! View pool configuration
show ip dhcp conflict           ! View IP conflicts
```

### DHCP Relay Agent
- DHCP uses broadcasts — routers do not forward broadcasts
- DHCP relay agent forwards DHCP broadcasts to a server
  on a different subnet
- Allows one DHCP server to serve multiple subnets

```plaintext
! Configure DHCP relay on router interface
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip helper-address 192.168.2.10
```

### APIPA — Automatic Private IP Addressing
- Used when DHCP server is unreachable
- Windows automatically assigns 169.254.x.x address
- Range: 169.254.0.0/16
- Not routable — local link only
- Seeing 169.254.x.x means DHCP has failed

### DHCP Security Issues
- **Rogue DHCP Server** — Unauthorized server hands out
  bad IP config (wrong gateway = MitM attack)
- **DHCP Starvation** — Attacker floods server with requests,
  exhausting the IP pool (DoS attack)
- **DHCP Snooping** — Cisco switch feature that blocks
  rogue DHCP servers on untrusted ports

```plaintext
! Enable DHCP snooping
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10
Switch(config)# interface FastEthernet0/1
Switch(config-if)# ip dhcp snooping trust  ! Trust uplink to router
```

## DNS vs DHCP Comparison

| Feature    | DNS                   | DHCP                        |
| ---------- | --------------------- | --------------------------- |
| Purpose    | Name to IP resolution | Automatic IP assignment     |
| OSI Layer  | Application (Layer 7) | Application (Layer 7)       |
| Protocol   | UDP/TCP port 53       | UDP ports 67/68             |
| Direction  | Client queries server | Client requests from server |
| Without it | Must use IP addresses | Must configure IPs manually |

## Key Exam Points
- DNS uses port 53 (UDP for queries, TCP for zone transfers)
- DHCP uses UDP port 67 (server) and 68 (client)
- DORA = Discover, Offer, Request, Acknowledge
- 169.254.x.x (APIPA) means DHCP failed
- A record = IPv4, AAAA record = IPv6
- DHCP relay agent (ip helper-address) forwards DHCP
  across subnets
- DHCP snooping protects against rogue DHCP servers
- DNS cache poisoning redirects users to malicious sites
- PTR records are used for reverse DNS lookups
- TTL controls how long DNS records are cached

## Related Notes
- [[IPv4 Addressing]]
- [[IPv6 Addressing]]
- [[Network Security]]
- [[Common Cisco CLI Commands]]
- [[CCST MOC]]