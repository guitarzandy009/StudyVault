
# Switching & VLANs

## Overview
Switches operate at OSI Layer 2 (Data Link) and forward frames
based on MAC addresses. VLANs (Virtual Local Area Networks)
logically segment a network into separate broadcast domains.

## Switching Fundamentals

### What is a Switch?
- Layer 2 device that forwards frames based on MAC addresses
- Replaces older hubs (which broadcast to all ports)
- Each port is its own collision domain
- Builds and maintains a MAC address table (CAM table)

### How a Switch Forwards Frames
1. Frame arrives on a switch port
2. Switch reads the **source MAC** and records it in the MAC table
3. Switch looks up the **destination MAC** in the MAC table
4. If found → forwards frame out the correct port (**unicast**)
5. If not found → floods frame out all ports except source (**unknown unicast flood**)
6. If broadcast → floods out all ports except source

### Switch MAC Address Table (CAM Table)
```plaintext
show mac address-table          ! View MAC address table

Mac Address Table
-----------------
Vlan  Mac Address       Type    Ports
----  -----------       ------  -----
1     00a1.2b3c.4d5e    DYNAMIC Fa0/1
1     00b2.3c4d.5e6f    DYNAMIC Fa0/2
10    00c3.4d5e.6f7a    DYNAMIC Fa0/3
```

### Switch vs Hub vs Router

| Feature          | Hub          | Switch          | Router          |
|------------------|--------------|-----------------|-----------------|
| OSI Layer        | Layer 1      | Layer 2         | Layer 3         |
| Forwarding basis | Broadcasts   | MAC addresses   | IP addresses    |
| Collision domain | One shared   | Per port        | Per port        |
| Broadcast domain | One shared   | One (per VLAN)  | Per interface   |
| Intelligence     | None         | MAC table       | Routing table   |

## VLANs — Virtual Local Area Networks

### What is a VLAN?
- Logical segmentation of a network at Layer 2
- Groups ports into separate broadcast domains
- Devices in different VLANs cannot communicate without a router
- Improves security, performance, and network management

### Why Use VLANs?
- **Security** — Isolates sensitive traffic (e.g. management, finance)
- **Performance** — Reduces broadcast traffic
- **Flexibility** — Group devices by function, not physical location
- **Cost** — Reduces need for additional physical switches

### Default VLAN
- VLAN 1 is the default VLAN on all Cisco switches
- All ports belong to VLAN 1 by default
- Best practice — do not use VLAN 1 for user traffic

### Reserved VLANs (Cisco)
| VLAN Range  | Use                              |
|-------------|----------------------------------|
| 1           | Default VLAN                     |
| 2-1001      | Normal range (user-defined)      |
| 1002-1005   | Legacy (FDDI, Token Ring)        |
| 1006-4094   | Extended range                   |

## VLAN Configuration (Cisco IOS)

### Creating a VLAN
```plaintext
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Engineering
Switch(config-vlan)# exit

Switch(config)# vlan 30
Switch(config-vlan)# name Management
Switch(config-vlan)# exit
```

### Assigning Ports to VLANs (Access Ports)
```plaintext
Switch(config)# interface FastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit
```

### Verifying VLANs
```plaintext
show vlan brief                 ! Summary of all VLANs
show vlan id 10                 ! Details of specific VLAN
show interfaces FastEthernet0/1 switchport ! Port VLAN info
```

## Trunk Ports

### What is a Trunk Port?
- Carries traffic for multiple VLANs between switches
- Used for switch-to-switch and switch-to-router links
- Tags frames with VLAN ID using 802.1Q encapsulation

### 802.1Q Tagging
- IEEE standard for VLAN tagging
- Inserts a 4-byte tag into the Ethernet frame
- Tag contains the VLAN ID (12 bits = up to 4094 VLANs)
- Native VLAN frames are sent untagged

### Trunk Configuration
```plaintext
Switch(config)# interface GigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 99
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Switch(config-if)# exit

show interfaces trunk           ! Verify trunk ports
```

## Native VLAN
- Traffic sent untagged on a trunk port
- Default native VLAN is VLAN 1
- Best practice — change native VLAN to an unused VLAN
- Native VLAN mismatch causes connectivity issues

## Inter-VLAN Routing
Devices in different VLANs need a Layer 3 device to communicate:

### Option 1 — Router on a Stick
- Single router interface with subinterfaces for each VLAN
- Each subinterface tagged with 802.1Q

```plaintext
Router(config)# interface GigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0

Router(config)# interface GigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
```

### Option 2 — Layer 3 Switch (SVI)
- Switch Virtual Interface (SVI) per VLAN
- More efficient than router on a stick

```plaintext
Switch(config)# ip routing
Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown

Switch(config)# interface vlan 20
Switch(config-if)# ip address 192.168.20.1 255.255.255.0
Switch(config-if)# no shutdown
```

## STP — Spanning Tree Protocol
- Prevents Layer 2 loops in redundant switch topologies
- Blocks redundant paths, activates them if primary fails
- IEEE 802.1D standard

### STP Port States
| State      | Description                              |
|------------|------------------------------------------|
| Blocking   | Receives BPDUs, does not forward frames  |
| Listening  | Preparing to forward, no MAC learning    |
| Learning   | Learning MAC addresses, not forwarding   |
| Forwarding | Fully operational, forwarding frames     |
| Disabled   | Administratively shut down              |

### RSTP — Rapid Spanning Tree Protocol
- IEEE 802.1W — faster convergence than STP
- Reduces convergence time from 30-50 seconds to seconds
- Most modern networks use RSTP

```plaintext
show spanning-tree              ! View STP status
show spanning-tree vlan 10      ! STP status for specific VLAN
```

## Key Exam Points
- Switches forward frames based on MAC addresses (Layer 2)
- Unknown unicast frames are flooded out all ports except source
- VLANs create separate broadcast domains
- Access ports carry traffic for one VLAN
- Trunk ports carry traffic for multiple VLANs using 802.1Q
- Native VLAN traffic is untagged on trunk ports
- Inter-VLAN routing requires a Layer 3 device
- STP prevents Layer 2 loops in redundant topologies
- VLAN 1 is the default — best practice is not to use it
- Layer 3 switches can route between VLANs using SVIs

## Related Notes
- [[OSI Model]]
- [[MAC Addresses & ARP]]
- [[IPv4 Addressing]]
- [[Routing Protocols]]
- [[Common Cisco CLI Commands]]
- [[CCST MOC]]