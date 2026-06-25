# PPP — Point-to-Point Protocol

## Overview
PPP is a Data Link layer protocol (OSI Layer 2) used to establish
a direct connection between two nodes over a serial link.
Commonly used in dial-up, DSL, and WAN connections.

## Key Features
- Encapsulates network layer packets for transmission
- Supports multiple network layer protocols (IP, IPX)
- Provides authentication, compression, and error detection
- Works over serial, phone lines, fiber, and Ethernet

## PPP Components

### LCP — Link Control Protocol
- Establishes, configures, and tests the data link connection
- Negotiates connection parameters before data transfer begins

### NCP — Network Control Protocol
- Configures network layer protocols after LCP establishes the link
- Each network protocol (IP, IPX) has its own NCP

## PPP Authentication Protocols

### PAP — Password Authentication Protocol
- Simple two-way handshake
- Sends username & password in **plaintext**
- Weak — vulnerable to interception
- Used only when security is not a concern

### CHAP — Challenge Handshake Authentication Protocol
- Three-way handshake
- Uses MD5 hashing — password never sent in plaintext
- More secure than PAP
- Periodically re-authenticates during the session

## PAP vs CHAP

| Feature        | PAP                  | CHAP                 |
|----------------|----------------------|----------------------|
| Handshake      | Two-way              | Three-way            |
| Password       | Sent in plaintext    | Never sent           |
| Security       | Weak                 | Strong               |
| Re-auth        | No                   | Yes (periodic)       |

## PPPoE — PPP over Ethernet
- Extends PPP over Ethernet networks
- Commonly used by ISPs for DSL broadband connections
- Allows authentication and encryption over Ethernet

## PPP Frame Structure
| Flag | Address | Control | Protocol | Data | FCS |
|------|---------|---------|----------|------|-----|
| 1B   | 1B      | 1B      | 2B       | Var  | 2-4B|

- **Flag** — Marks start/end of frame (0x7E)
- **Protocol** — Identifies the network layer protocol
- **FCS** — Frame Check Sequence for error detection

## Key Exam Points
- PPP operates at OSI Layer 2 (Data Link)
- CHAP is more secure than PAP
- PPPoE is widely used for DSL broadband
- LCP sets up the link, NCP handles network protocols
- PPP supports authentication, compression, and error detection

## Related Notes
- [[OSI Model]]
- [[TCP-IP Model]]
- [[WAN Technologies]]
- [[CCST MOC]]