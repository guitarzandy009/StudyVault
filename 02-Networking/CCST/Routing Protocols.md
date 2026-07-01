
# Routing Protocols

## Overview
Routing is the process of forwarding packets between networks
at OSI Layer 3. Routers use routing protocols to dynamically
discover and maintain paths to destination networks.

## Types of Routing

### Static Routing
- Manually configured by an administrator
- Does not adapt to network changes
- Low overhead, good for small networks
- Used for default routes and stub networks

```plaintext
ip route 192.168.2.0 255.255.255.0 192.168.1.254  ! Static route
ip route 0.0.0.0 0.0.0.0 192.168.1.254            ! Default route
show ip route static                                ! View static routes
```

### Dynamic Routing
- Routers automatically discover and share routes
- Adapts to network changes (link failures, new networks)
- Requires more CPU and bandwidth than static routing
- Scales well for large networks

### Default Route
- Gateway of last resort
- Used when no specific route matches destination
- 0.0.0.0/0 matches all destinations

## Routing Table
```plaintext
show ip route

Codes:
C - Connected
S - Static
O - OSPF
R - RIP
D - EIGRP
B - BGP
* - Candidate default

Gateway of last resort is 192.168.1.254 to network 0.0.0.0

C    192.168.1.0/24 is directly connected, GigabitEthernet0/0
S    192.168.2.0/24 [1/0] via 192.168.1.254
O    192.168.3.0/24 [110/2] via 192.168.1.254
```

## Administrative Distance (AD)
Used to select the best route when multiple protocols
know about the same destination. Lower AD = more trusted.

| Route Source      | Administrative Distance |
| ----------------- | ----------------------- |
| Connected         | 0                       |
| Static            | 1                       |
| EIGRP (internal)  | 90                      |
| OSPF              | 110                     |
| RIP               | 120                     |
| EIGRP (external)  | 170                     |
| Unknown/Untrusted | 255                     |

## Routing Protocol Classifications

### By Update Method
- **Distance Vector** — Shares routing table with neighbors
  (RIP, EIGRP)
- **Link State** — Shares link state info with all routers
  (OSPF, IS-IS)
- **Path Vector** — Shares path attributes (BGP)

### By Scope
- **IGP (Interior Gateway Protocol)** — Within an autonomous system
  (RIP, OSPF, EIGRP)
- **EGP (Exterior Gateway Protocol)** — Between autonomous systems
  (BGP)

## RIP — Routing Information Protocol

### Overview
- Distance vector protocol
- Uses hop count as metric (max 15 hops)
- 16 hops = unreachable
- Sends full routing table every 30 seconds
- Slow convergence
- Best for small, simple networks

### Versions
| Feature        | RIPv1 | RIPv2           |
| -------------- | ----- | --------------- |
| Classful       | Yes   | No (CIDR)       |
| Authentication | No    | Yes (MD5)       |
| Multicast      | No    | Yes (224.0.0.9) |
| VLSM support   | No    | Yes             |

### RIP Configuration
```plaintext
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 192.168.1.0
Router(config-router)# network 192.168.2.0
Router(config-router)# no auto-summary
Router(config-router)# passive-interface GigabitEthernet0/1

show ip rip database            ! View RIP database
show ip protocols               ! View routing protocol info
```

## OSPF — Open Shortest Path First

### Overview
- Link state protocol
- Uses Dijkstra's SPF algorithm
- Metric = cost (based on bandwidth)
- Faster convergence than RIP
- Scales well for large networks
- Industry standard IGP for enterprise networks
- Key protocol for CCST and CCNA exams

### OSPF Key Concepts
- **Area** — Logical grouping of routers
- **Area 0** — Backbone area, all areas must connect to it
- **LSA** — Link State Advertisement, shares topology info
- **LSDB** — Link State Database, map of the network
- **SPF Tree** — Shortest path calculated from LSDB
- **DR/BDR** — Designated Router / Backup DR on multi-access networks

### OSPF Router Types
| Type              | Description                              |
| ----------------- | ---------------------------------------- |
| Internal Router   | All interfaces in one area               |
| Backbone Router   | At least one interface in Area 0         |
| ABR (Area Border) | Connects multiple areas                  |
| ASBR              | Connects OSPF to external routing domain |

### OSPF Neighbor States
| State    | Description                             |
| -------- | --------------------------------------- |
| Down     | No hellos received                      |
| Init     | Hello received, not bidirectional       |
| Two-Way  | Bidirectional communication established |
| ExStart  | Master/slave relationship negotiated    |
| Exchange | DBD packets exchanged                   |
| Loading  | LSR/LSU packets exchanged               |
| Full     | Fully adjacent, synchronized LSDB       |

### OSPF Configuration
```plaintext
Router(config)# router ospf 1
Router(config-router)# router-id 1.1.1.1
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router(config-router)# network 192.168.2.0 0.0.0.255 area 0
Router(config-router)# passive-interface GigabitEthernet0/1

show ip ospf neighbor           ! View OSPF neighbors
show ip ospf database           ! View LSDB
show ip ospf interface          ! View OSPF interface info
show ip protocols               ! View routing protocol info
debug ip ospf events            ! Debug OSPF events
```

### OSPF Cost Calculation
- Cost = Reference bandwidth / Interface bandwidth
- Default reference bandwidth = 100 Mbps
- FastEthernet (100Mbps) cost = 1
- GigabitEthernet (1Gbps) cost = 1 (same as FastEthernet by default)
- Best practice — change reference bandwidth to 1000 or 10000

```plaintext
Router(config-router)# auto-cost reference-bandwidth 1000
```

## EIGRP — Enhanced Interior Gateway Routing Protocol

### Overview
- Advanced distance vector (hybrid) protocol
- Cisco proprietary (now partially open)
- Uses DUAL algorithm (Diffusing Update Algorithm)
- Metric based on bandwidth and delay
- Very fast convergence
- Supports VLSM and CIDR

### EIGRP Key Terms
- **Successor** — Best path to destination
- **Feasible Successor** — Backup path, pre-calculated
- **Feasible Distance (FD)** — Best metric to destination
- **Reported Distance (RD)** — Neighbor's metric to destination

### EIGRP Configuration
```plaintext
Router(config)# router eigrp 100
Router(config-router)# network 192.168.1.0 0.0.0.255
Router(config-router)# network 192.168.2.0 0.0.0.255
Router(config-router)# no auto-summary

show ip eigrp neighbors         ! View EIGRP neighbors
show ip eigrp topology          ! View EIGRP topology table
show ip eigrp interfaces        ! View EIGRP interfaces
```

## BGP — Border Gateway Protocol

### Overview
- Path vector protocol
- Used between autonomous systems (AS) on the internet
- The routing protocol of the internet
- Very slow convergence but highly scalable
- Not typically configured in CCST — more relevant for CCNA/CCNP

## Routing Protocol Comparison

| Feature     | RIP          | OSPF         | EIGRP      | BGP         |
| ----------- | ------------ | ------------ | ---------- | ----------- |
| Type        | Distance     | Link State   | Hybrid     | Path Vector |
| Algorithm   | Bellman-Ford | Dijkstra SPF | DUAL       | Best Path   |
| Metric      | Hop count    | Cost         | BW + Delay | Attributes  |
| AD          | 120          | 110          | 90         | 20/200      |
| Max hops    | 15           | Unlimited    | 255        | Unlimited   |
| Convergence | Slow         | Fast         | Very Fast  | Very Slow   |
| Scope       | IGP          | IGP          | IGP        | EGP         |
| Standard    | Open         | Open         | Cisco      | Open        |

## Key Exam Points
- Administrative distance determines most trusted route source
- Lower AD = more preferred route
- Connected routes always preferred (AD 0)
- OSPF uses cost as metric, RIP uses hop count
- OSPF Area 0 is the backbone — mandatory
- RIP maximum hop count is 15 (16 = unreachable)
- EIGRP is Cisco proprietary
- - BGP is the inter-domain routing protocol that connects autonomous
  systems across the internet — used by ISPs and large enterprises,
  not typical internal networks
- Static routes have AD of 1 — preferred over dynamic routes
- Default route is 0.0.0.0/0 — gateway of last resort
- passive-interface stops routing updates on that interface

## Related Notes
- [[OSI Model]]
- [[IPv4 Addressing]]
- [[Subnetting]]
- [[Switching & VLANs]]
- [[Common Cisco CLI Commands]]
- [[CCST MOC]]