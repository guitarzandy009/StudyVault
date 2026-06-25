# TCP/IP Model

## Overview
The TCP/IP model is the practical framework used on real networks
and the internet. It compresses the OSI model into 4 layers.

## The 4 Layers

### Layer 4 — Application
- Combines OSI Layers 5, 6, 7
- Handles data representation, sessions, and app services
- Protocols: HTTP, HTTPS, FTP, SMTP, DNS, DHCP, SSH, Telnet, SNMP

### Layer 3 — Transport
- Same as OSI Layer 4
- End-to-end communication between hosts
- Protocols:
  - **TCP** — Transmission Control Protocol (reliable, connection-oriented)
  - **UDP** — User Datagram Protocol (fast, connectionless)

### Layer 2 — Internet
- Equivalent to OSI Layer 3
- Logical addressing & routing packets across networks
- Protocols: IP, ICMP, ARP, OSPF

### Layer 1 — Network Access
- Combines OSI Layers 1 & 2
- Physical transmission & MAC addressing
- Protocols: Ethernet, Wi-Fi (802.11), PPP

## OSI vs TCP/IP Comparison

| OSI Layer | OSI Name     | TCP/IP Layer    |
|-----------|--------------|-----------------|
| 7         | Application  | Application     |
| 6         | Presentation | Application     |
| 5         | Session      | Application     |
| 4         | Transport    | Transport       |
| 3         | Network      | Internet        |
| 2         | Data Link    | Network Access  |
| 1         | Physical     | Network Access  |

## TCP vs UDP

| Feature        | TCP                  | UDP                  |
|----------------|----------------------|----------------------|
| Connection     | Connection-oriented  | Connectionless       |
| Reliability    | Reliable             | Unreliable           |
| Speed          | Slower               | Faster               |
| Error checking | Yes                  | Minimal              |
| Use cases      | HTTP, FTP, SSH       | DNS, VoIP, Streaming |

## TCP Three-Way Handshake
Used to establish a TCP connection:
1. **SYN** — Client sends synchronize request
2. **SYN-ACK** — Server acknowledges and responds
3. **ACK** — Client acknowledges, connection established

## Key Exam Points
- TCP/IP is the model used on the actual internet
- OSI is the reference model used for troubleshooting
- TCP is reliable but slower — UDP is fast but no guarantee of delivery
- ICMP is used for diagnostics (ping, traceroute) at the Internet layer
- ARP resolves IP addresses to MAC addresses

## Related Notes
- [[OSI Model]]
- [[IPv4 Addressing]]
- [[CCST MOC]]