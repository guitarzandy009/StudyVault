
# Common Cisco CLI Commands

## Overview
Cisco IOS (Internetwork Operating System) is the operating system
used on Cisco routers and switches. Commands are entered via the
CLI (Command Line Interface) in Packet Tracer or on real hardware.

## CLI Modes

| Mode                | Prompt          | Access Command      |
|---------------------|-----------------|---------------------|
| User EXEC           | Router>         | (default on login)  |
| Privileged EXEC     | Router#         | enable              |
| Global Config       | Router(config)# | configure terminal  |
| Interface Config    | Router(config-if)# | interface [type] |
| Line Config         | Router(config-line)# | line [type]    |

## Navigating Modes
```plaintext
enable                    ! Enter Privileged EXEC
configure terminal        ! Enter Global Config (from Privileged EXEC)
exit                      ! Go back one mode
end                       ! Return directly to Privileged EXEC
Ctrl+Z                    ! Return directly to Privileged EXEC
```

## Essential Show Commands
```plaintext
show version              ! IOS version, uptime, hardware info
show running-config       ! Current active configuration
show startup-config       ! Saved configuration in NVRAM
show ip interface brief   ! Summary of all interfaces and IP addresses
show interfaces           ! Detailed interface statistics
show ip route             ! Routing table
show mac address-table    ! MAC address table (switches)
show vlan brief           ! VLAN summary (switches)
show cdp neighbors        ! Directly connected Cisco devices
show clock                ! Current date and time
```

## Basic Device Configuration
```plaintext
hostname Router1          ! Set device hostname
enable secret cisco123    ! Set encrypted privileged EXEC password
service password-encryption ! Encrypt all plaintext passwords
banner motd # Authorized Access Only # ! Set login banner
no ip domain-lookup       ! Disable DNS lookup on mistyped commands
```

## Interface Configuration
```plaintext
interface GigabitEthernet0/0      ! Enter interface config
 ip address 192.168.1.1 255.255.255.0  ! Assign IP address
 no shutdown                      ! Enable the interface
 shutdown                         ! Disable the interface
 description Link to LAN          ! Add interface description
 duplex full                      ! Set duplex mode
 speed 100                        ! Set interface speed
exit                              ! Exit interface config
```

## Saving Configuration
```plaintext
copy running-config startup-config  ! Save config to NVRAM
write memory                        ! Shorthand save command
erase startup-config                ! Wipe saved config
reload                              ! Reboot the device
```

## Routing Commands
```plaintext
ip route 0.0.0.0 0.0.0.0 192.168.1.254   ! Default static route
ip route 10.0.0.0 255.255.255.0 192.168.1.1 ! Static route
router ospf 1                              ! Enable OSPF process 1
 network 192.168.1.0 0.0.0.255 area 0     ! Advertise network
show ip protocols                          ! Show routing protocols
show ip ospf neighbor                      ! Show OSPF neighbors
```

## Switch-Specific Commands
```plaintext
show mac address-table          ! View MAC address table
show vlan brief                 ! View VLAN assignments
vlan 10                         ! Create VLAN 10
 name Sales                     ! Name the VLAN
interface FastEthernet0/1
 switchport mode access         ! Set port as access port
 switchport access vlan 10      ! Assign port to VLAN 10
interface FastEthernet0/24
 switchport mode trunk          ! Set port as trunk port
```

## Console & Remote Access
```plaintext
line console 0
 password cisco                 ! Set console password
 login                          ! Enable password prompt
line vty 0 4
 password cisco                 ! Set Telnet/SSH password
 login                          ! Enable password prompt
 transport input ssh            ! Allow SSH only (no Telnet)
ip domain-name lab.local        ! Required for SSH
crypto key generate rsa         ! Generate RSA keys for SSH
ip ssh version 2                ! Use SSH version 2
username admin secret cisco123  ! Create local user account
```

## Troubleshooting Commands
```plaintext
ping 192.168.1.1                ! Test connectivity
traceroute 192.168.1.1          ! Trace path to destination
show ip interface brief         ! Check interface status
debug ip icmp                   ! Debug ICMP packets (use carefully)
undebug all                     ! Turn off all debugging
show logging                    ! View system log messages
```

## Interface Status Cheat Sheet

| Status                    | Line Protocol | Meaning                        |
|---------------------------|---------------|--------------------------------|
| up                        | up            | Fully operational              |
| up                        | down          | Physical OK, Layer 2 issue     |
| down                      | down          | Physical layer problem         |
| administratively down     | down          | Manually shut down             |

## IOS Keyboard Shortcuts

| Shortcut     | Action                              |
|--------------|-------------------------------------|
| Tab          | Auto-complete command               |
| ?            | List available commands             |
| Ctrl+C       | Abort current command               |
| Ctrl+Z       | Return to Privileged EXEC           |
| Ctrl+Shift+6 | Interrupt ping or traceroute        |
| Up Arrow     | Recall previous command             |

## Key Exam Points
- Always use `no shutdown` to bring up an interface
- `show ip interface brief` is your go-to troubleshooting command
- `copy running-config startup-config` saves your config — forgetting
  this means changes are lost on reboot
- `enable secret` is encrypted — always prefer over `enable password`
- administratively down means someone ran `shutdown` on the interface
- `no ip domain-lookup` prevents frustrating DNS delays on typos

## Related Notes
- [[OSI Model]]
- [[IPv4 Addressing]]
- [[Subnetting]]
- [[Packet Tracer Labs]]
- [[CCST MOC]]