# OSI Model

## Overview
The OSI (Open Systems Interconnection) model is a conceptual framework
that standardizes network communication into 7 layers.

## The 7 Layers

### Layer 7 — Application
- Closest to the end user
- Provides network services to applications
- Protocols: HTTP, HTTPS, FTP, SMTP, DNS, DHCP, SSH, Telnet

### Layer 6 — Presentation
- Data translation, encryption, compression
- Converts data formats (e.g. JPEG, ASCII, SSL/TLS)

### Layer 5 — Session
- Manages sessions between applications
- Establishes, maintains, and terminates connections
- Protocols: NetBIOS, RPC

### Layer 4 — Transport
- End-to-end communication & error recovery
- Segmentation & reassembly
- Protocols: TCP (reliable), UDP (fast, unreliable)
- Key concepts: ports, flow control, error checking

### Layer 3 — Network
- Logical addressing & routing
- Protocols: IP, ICMP, OSPF, RIP
- Devices: Routers

### Layer 2 — Data Link
- Physical addressing (MAC addresses)
- Frame creation & error detection
- Protocols: Ethernet, Wi-Fi (802.11), PPP
- Devices: Switches, Bridges

### Layer 1 — Physical
- Raw bit transmission over physical medium
- Cables, connectors, voltage levels, fiber optics
- Devices: Hubs, repeaters, cables

## Memory Aid
**All People Seem To Need Data Processing**
(Application → Presentation → Session → Transport → Network → Data Link → Physical)

## Key Exam Points
- TCP/IP model compresses OSI into 4 layers
- Routers operate at Layer 3
- Switches operate at Layer 2
- MAC addresses live at Layer 2
- IP addresses live at Layer 3

## Related Notes
- [[TCP-IP Model]]
- [[CCST MOC]]