# deep-in-net

A networking project using Cisco Packet Tracer covering fundamental networking concepts including OSI model, IP addressing, subnetting, routing, and key protocols.

---

## Table of Contents
- [Exercise 1 - Direct PC Connections](#exercise-1---direct-pc-connections)
- [Exercise 2 - Switch and Hub](#exercise-2---switch-and-hub)
- [Exercise 3 - Servers (DHCP, DNS, HTTPS, FTP)](#exercise-3---servers)
- [Exercise 4 - Router and Default Gateway](#exercise-4---router-and-default-gateway)
- [Exercise 5 - Multiple Subnets](#exercise-5---multiple-subnets)
- [Exercise 6 - Routing Table and Static Routes](#exercise-6---routing-table-and-static-routes)
- [Exercise 7 - Multi-Subnet with Serial Link](#exercise-7---multi-subnet-with-serial-link)
- [Exercise 8 - Three Subnets with Multiple Routers](#exercise-8---three-subnets-with-multiple-routers)
- [Knowledge Reference](#knowledge-reference)

---

## Exercise 1 - Direct PC Connections

### Objective
Connect 3 pairs of PCs directly to each other using crossover cables.

### Topology
```
PC0 <----crossover----> PC1
PC2 <----crossover----> PC3
PC4 <----crossover----> PC5
```

### IP Configuration
| Device | IP Address | Subnet Mask |
|--------|-----------|-------------|
| PC0 | 192.168.1.10 | 255.255.255.0 |
| PC1 | 192.168.1.11 | 255.255.255.0 |
| PC2 | 192.168.2.10 | 255.255.255.0 |
| PC3 | 192.168.2.11 | 255.255.255.0 |
| PC4 | 192.168.3.10 | 255.255.255.0 |
| PC5 | 192.168.3.11 | 255.255.255.0 |

### Subnet Calculations
Each pair uses a /24 subnet:
```
Block size = 2^8 = 256
Usable hosts = 256 - 2 = 254
Mask = 255.255.255.0
```

### Cable Type
Crossover cable used for PC to PC (same device type).

### Knowledge

**What is an RJ-45 cable?**
RJ-45 (Registered Jack 45) is the standard network connector with 8 pins and 4 pairs of twisted copper wires used to connect networking devices.

**Straight-Through vs Crossover:**
- Straight-Through: both ends identical wiring — used for different device types (PC→Switch, Switch→Router)
- Crossover: ends have opposite wiring (TX/RX swapped) — used for same device types (PC→PC, Switch→Switch)

**How IP addresses are calculated:**
```
Step 1: host bits = 32 - prefix (e.g. /24 → 8 host bits)
Step 2: block size = 2^host bits (e.g. 2^8 = 256)
Step 3: mask = 256 - block size (e.g. 256 - 256 = 0 → 255.255.255.0)
Step 4: usable hosts = block size - 2
Step 5: broadcast = network address + block size - 1
```

---

## Exercise 2 - Switch and Hub

### Objective
Connect multiple PCs to a switch and a hub, demonstrating the difference in behavior.

### Topology
```
S-PC1 ─┐
S-PC2 ─┤── Switch (192.168.1.0/29)
S-PC3 ─┤
S-PC4 ─┤
S-PC5 ─┘

H-PC1 ─┐
H-PC2 ─┤── Hub (192.168.1.192/27)
H-PC3 ─┤
H-PC4 ─┤
H-PC5 ─┘
```

### IP Configuration
| Device | IP Address | Subnet Mask |
|--------|-----------|-------------|
| S-PC1 | 192.168.1.1 | 255.255.255.248 |
| S-PC2 | 192.168.1.2 | 255.255.255.248 |
| S-PC3 | 192.168.1.3 | 255.255.255.248 |
| S-PC4 | 192.168.1.4 | 255.255.255.248 |
| S-PC5 | 192.168.1.5 | 255.255.255.248 |
| H-PC1 | 192.168.1.193 | 255.255.255.224 |
| H-PC2 | 192.168.1.194 | 255.255.255.224 |
| H-PC3 | 192.168.1.195 | 255.255.255.224 |
| H-PC4 | 192.168.1.196 | 255.255.255.224 |
| H-PC5 | 192.168.1.197 | 255.255.255.224 |

### Cable Type
Straight-Through for all PC → Switch and PC → Hub connections.

### Knowledge

**Switch (Layer 2 - Data Link):**
A switch learns MAC addresses of connected devices and builds a MAC address table. It sends data only to the intended destination port — not to everyone. This makes it efficient and fast.

**Hub (Layer 1 - Physical):**
A hub has no intelligence. It broadcasts every received packet to ALL connected ports. Every device receives every packet even if it was not the target. This causes collisions and wastes bandwidth.

**Differences:**
| Feature | Hub | Switch |
|---------|-----|--------|
| OSI Layer | Layer 1 | Layer 2 |
| Intelligence | None | Learns MAC addresses |
| Sends data to | ALL devices | Only target device |
| Collisions | Yes | No |

---

## Exercise 3 - Servers

### Objective
Configure DHCP, DNS, HTTPS, and FTP servers on a network with 6 PCs.

### Topology
```
HTTPS-Server (192.168.1.99) ─┐
FTP-Server   (192.168.1.100) ─┤
DNS-Server   (192.168.1.101) ─┼── Switch ── PC0~PC4 (DHCP)
DHCP-Server                  ─┤             PC5 (192.168.1.7 static)
```

### IP Configuration
| Device | IP Address | Subnet Mask | Type |
|--------|-----------|-------------|------|
| HTTPS Server | 192.168.1.99 | 255.255.255.0 | Static |
| FTP Server | 192.168.1.100 | 255.255.255.0 | Static |
| DNS Server | 192.168.1.101 | 255.255.255.0 | Static |
| PC5 | 192.168.1.7 | 255.255.255.0 | Static |
| PC0-PC4 | 192.168.1.10+ | 255.255.255.0 | DHCP |

### Server Configurations

**DHCP Server:**
- Pool start: 192.168.1.10
- DNS Server field: 192.168.1.101
- Subnet Mask: 255.255.255.0

**HTTPS Server:**
- HTTP: OFF
- HTTPS: ON
- Page displays "Hello" message

**FTP Server:**
- User: deepinnet
- Permissions: Read, Write, Delete, Rename, List (RWDNL)

**DNS Server:**
```
deep-in-net.local → 192.168.1.99    (A Record)
deep-in-net.com   → deep-in-net.local (CNAME Record)
```

### Knowledge

**DHCP** (Dynamic Host Configuration Protocol): Automatically assigns IPs via DORA process (Discover, Offer, Request, Acknowledge). Port 67/68, UDP, Layer 7.

**DNS** (Domain Name System): Translates domain names to IP addresses. Port 53, UDP, Layer 7.
- A Record: name → IPv4 address
- CNAME: name → another name (alias)
- MX: mail server
- AAAA: name → IPv6 address
- PTR: reverse lookup

**HTTP vs HTTPS:**
| HTTP | HTTPS |
|------|-------|
| Port 80 | Port 443 |
| No encryption | SSL/TLS encrypted |
| Layer 7 | Layer 7 |

**FTP** (File Transfer Protocol): Transfers files. Port 21 (control), Port 20 (data). TCP, Layer 7.

**TCP vs UDP:**
| TCP | UDP |
|-----|-----|
| Reliable, guaranteed delivery | Fast, no guarantee |
| Connection required | No connection |
| HTTP, HTTPS, FTP | DNS, DHCP |
| Layer 4 | Layer 4 |

**Ports reference:**
| Protocol | Port | Layer | Protocol Type |
|----------|------|-------|---------------|
| DHCP | 67/68 | 7 | UDP |
| DNS | 53 | 7 | UDP |
| HTTP | 80 | 7 | TCP |
| HTTPS | 443 | 7 | TCP |
| FTP | 20/21 | 7 | TCP |

---

## Exercise 4 - Router and Default Gateway

### Objective
Connect two PCs on different networks using a router.

### Topology
```
PC0 (192.168.1.2/30) ── Router ── PC1 (192.168.2.2/30)
```

### IP Configuration
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|-----------|-------------|
| PC0 | Fa0 | 192.168.1.2 | 255.255.255.252 |
| Router | Fa0/0 | 192.168.1.1 | 255.255.255.252 |
| Router | Fa0/1 | 192.168.2.1 | 255.255.255.252 |
| PC1 | Fa0 | 192.168.2.2 | 255.255.255.252 |

### Subnet Calculations (/30)
```
host bits = 32 - 30 = 2
block size = 2^2 = 4
mask = 256 - 4 = 252 → 255.255.255.252
usable hosts = 4 - 2 = 2 (perfect for 1 router + 1 PC)
```

### Router Configuration
```
enable
configure terminal
interface fastethernet0/0
ip address 192.168.1.1 255.255.255.252
no shutdown
exit
interface fastethernet0/1
ip address 192.168.2.1 255.255.255.252
no shutdown
exit
end
write memory
```

### Cable Type
Crossover for PC → Router connections.

### Knowledge

**Router (Layer 3 - Network):**
Connects different networks together using IP addresses and routing tables. Forwards packets toward their destination by finding the best path.

**Switch vs Router:**
| Feature | Switch | Router |
|---------|--------|--------|
| OSI Layer | Layer 2 | Layer 3 |
| Uses | MAC addresses | IP addresses |
| Connects | Same network devices | Different networks |

**Default Gateway:**
The IP address of the router interface on your local subnet. When a device wants to reach a different network it sends traffic to the default gateway which routes it forward.

---

## Exercise 5 - Multiple Subnets

### Objective
Connect two groups of PCs on different subnets through a single router.

### Topology
```
PC1-PC5 ── Switch1 ── Router ── Switch2 ── PC6-PC10
(/29 subnet)                    (/27 subnet)
```

### IP Configuration
| Device | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|-------------|-----------------|
| Router Fa0/0 | 192.168.1.1 | 255.255.255.248 | — |
| PC1 | 192.168.1.2 | 255.255.255.248 | 192.168.1.1 |
| PC2 | 192.168.1.3 | 255.255.255.248 | 192.168.1.1 |
| PC3 | 192.168.1.4 | 255.255.255.248 | 192.168.1.1 |
| PC4 | 192.168.1.5 | 255.255.255.248 | 192.168.1.1 |
| PC5 | 192.168.1.6 | 255.255.255.248 | 192.168.1.1 |
| Router Fa0/1 | 192.168.1.193 | 255.255.255.224 | — |
| PC6 | 192.168.1.194 | 255.255.255.224 | 192.168.1.193 |
| PC7 | 192.168.1.195 | 255.255.255.224 | 192.168.1.193 |
| PC8 | 192.168.1.196 | 255.255.255.224 | 192.168.1.193 |
| PC9 | 192.168.1.197 | 255.255.255.224 | 192.168.1.193 |
| PC10 | 192.168.1.198 | 255.255.255.224 | 192.168.1.193 |

### Subnet Calculations

**Subnet 1 (/29):**
```
host bits = 32 - 29 = 3
block size = 2^3 = 8
mask = 256 - 8 = 248 → 255.255.255.248
usable hosts = 8 - 2 = 6
Network: 192.168.1.0 | Broadcast: 192.168.1.7
```

**Subnet 2 (/27):**
```
host bits = 32 - 27 = 5
block size = 2^5 = 32
mask = 256 - 32 = 224 → 255.255.255.224
usable hosts = 32 - 2 = 30
Network: 192.168.1.192 | Broadcast: 192.168.1.223
```

---

## Exercise 6 - Routing Table and Static Routes

### Objective
Connect two PCs through two routers using a serial link and static routes.

### Topology
```
PC1 (192.168.1.2/24) ── Router1 ══serial══ Router2 ── PC2 (192.168.2.2/24)
                        10.10.0.1/30        10.10.0.2/30
```

### IP Configuration
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|-----------|-------------|
| PC1 | Fa0 | 192.168.1.2 | 255.255.255.0 |
| Router1 | Fa0/0 | 192.168.1.1 | 255.255.255.0 |
| Router1 | Serial2/0 | 10.10.0.1 | 255.255.255.252 |
| Router2 | Serial2/0 | 10.10.0.2 | 255.255.255.252 |
| Router2 | Fa0/0 | 192.168.2.1 | 255.255.255.0 |
| PC2 | Fa0 | 192.168.2.2 | 255.255.255.0 |

### Router Configuration

**Router1:**
```
enable
configure terminal
interface fastethernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface serial2/0
ip address 10.10.0.1 255.255.255.252
clock rate 64000
no shutdown
exit
ip route 192.168.2.0 255.255.255.0 10.10.0.2
end
write memory
```

**Router2:**
```
enable
configure terminal
interface serial2/0
ip address 10.10.0.2 255.255.255.252
no shutdown
exit
interface fastethernet0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
ip route 192.168.1.0 255.255.255.0 10.10.0.1
end
write memory
```

### Knowledge

**Routing Table:**
A map stored inside the router that tells it where to send packets for each network.
```
show ip route

C = Connected    (router knows directly via its interface)
S = Static       (manually added by administrator)
R = RIP          (learned automatically from another router)
```

**Static Route Syntax:**
```
ip route [destination network] [subnet mask] [next hop IP]

Example:
ip route 192.168.2.0 255.255.255.0 10.10.0.2
         ↑ destination  ↑ mask      ↑ next router IP
```

**Rule:** Next hop = the neighboring router's IP on the cable between them — never your own interface IP.

---

## Exercise 7 - Multi-Subnet with Serial Link

### Objective
Connect two groups of PCs on different subnets through two routers with a serial link.

### Topology
```
PC1-PC5 ── Switch1 ── Router1 ══serial══ Router2 ── Switch2 ── Laptop0,PC6-PC8
           (/24)      10.10.0.1/30  10.10.0.2/30    (/24)
```

### IP Configuration
| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|-----------|-------------|-----------------|
| Router1 | Fa0/0 | 192.168.1.1 | 255.255.255.0 | — |
| Router1 | Serial2/0 | 10.10.0.1 | 255.255.255.252 | — |
| Router2 | Serial2/0 | 10.10.0.2 | 255.255.255.252 | — |
| Router2 | Fa0/0 | 192.168.2.1 | 255.255.255.0 | — |
| PC1 | Fa0 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC2 | Fa0 | 192.168.1.3 | 255.255.255.0 | 192.168.1.1 |
| PC3 | Fa0 | 192.168.1.4 | 255.255.255.0 | 192.168.1.1 |
| PC4 | Fa0 | 192.168.1.5 | 255.255.255.0 | 192.168.1.1 |
| PC5 | Fa0 | 192.168.1.6 | 255.255.255.0 | 192.168.1.1 |
| Laptop0 | Fa0 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC6 | Fa0 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 |
| PC7 | Fa0 | 192.168.2.4 | 255.255.255.0 | 192.168.2.1 |
| PC8 | Fa0 | 192.168.2.5 | 255.255.255.0 | 192.168.2.1 |

### Router Configuration

**Router1:**
```
enable
configure terminal
interface fastethernet0/0
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface serial2/0
ip address 10.10.0.1 255.255.255.252
clock rate 64000
no shutdown
exit
ip route 192.168.2.0 255.255.255.0 10.10.0.2
end
write memory
```

**Router2:**
```
enable
configure terminal
interface serial2/0
ip address 10.10.0.2 255.255.255.252
no shutdown
exit
interface fastethernet0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
ip route 192.168.1.0 255.255.255.0 10.10.0.1
end
write memory
```

### Static Routes Logic
```
Router1 knows: 192.168.1.0/24, 10.10.0.0/30
Router1 needs: 192.168.2.0/24 → send to 10.10.0.2

Router2 knows: 10.10.0.0/30, 192.168.2.0/24
Router2 needs: 192.168.1.0/24 → send to 10.10.0.1
```

---

## Exercise 8 - Three Subnets with Multiple Routers

### Objective
Connect three groups of PCs across three routers with two serial links, ensuring full communication between all subnets.

### Topology
```
PC1-PC5 ── Switch1 ── Router1 ══serial══ Router2 ══serial══ Router3 ── Switch3 ── PC9-PC11
           (/26)      10.10.0.1/30  10.10.0.2/30  10.10.1.1/30  10.10.1.2/30    (/28)
                                         |
                                      Switch2
                                       (/24)
                                  Laptop1,PC6-PC8
```

### IP Configuration

**Subnet 1 - 192.168.1.192/26:**
| Device | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|-------------|-----------------|
| Router1 Fa0/0 | 192.168.1.193 | 255.255.255.192 | — |
| PC1 | 192.168.1.194 | 255.255.255.192 | 192.168.1.193 |
| PC2 | 192.168.1.195 | 255.255.255.192 | 192.168.1.193 |
| PC3 | 192.168.1.196 | 255.255.255.192 | 192.168.1.193 |
| PC4 | 192.168.1.197 | 255.255.255.192 | 192.168.1.193 |
| PC5 | 192.168.1.198 | 255.255.255.192 | 192.168.1.193 |

**Serial Links:**
| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|-----------|-------------|
| Router1 | Serial2/0 | 10.10.0.1 | 255.255.255.252 |
| Router2 | Serial2/0 | 10.10.0.2 | 255.255.255.252 |
| Router2 | Serial3/0 | 10.10.1.1 | 255.255.255.252 |
| Router3 | Serial2/0 | 10.10.1.2 | 255.255.255.252 |

**Subnet 2 - 192.168.2.0/24:**
| Device | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|-------------|-----------------|
| Router2 Fa0/0 | 192.168.2.1 | 255.255.255.0 | — |
| Laptop1 | 192.168.2.2 | 255.255.255.0 | 192.168.2.1 |
| PC6 | 192.168.2.3 | 255.255.255.0 | 192.168.2.1 |
| PC7 | 192.168.2.4 | 255.255.255.0 | 192.168.2.1 |
| PC8 | 192.168.2.5 | 255.255.255.0 | 192.168.2.1 |

**Subnet 3 - 192.168.3.160/28:**
| Device | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|-------------|-----------------|
| Router3 Fa0/0 | 192.168.3.161 | 255.255.255.240 | — |
| PC9 | 192.168.3.162 | 255.255.255.240 | 192.168.3.161 |
| PC10 | 192.168.3.163 | 255.255.255.240 | 192.168.3.161 |
| PC11 | 192.168.3.164 | 255.255.255.240 | 192.168.3.161 |

### Subnet Calculations

**Subnet 1 (/26):**
```
host bits = 32 - 26 = 6
block size = 2^6 = 64
mask = 256 - 64 = 192 → 255.255.255.192
usable hosts = 64 - 2 = 62
Network: 192.168.1.192 | Broadcast: 192.168.1.255
```

**Subnet 2 (/24):**
```
host bits = 32 - 24 = 8
block size = 2^8 = 256
mask = 255.255.255.0
usable hosts = 256 - 2 = 254
Network: 192.168.2.0 | Broadcast: 192.168.2.255
```

**Subnet 3 (/28):**
```
host bits = 32 - 28 = 4
block size = 2^4 = 16
mask = 256 - 16 = 240 → 255.255.255.240
usable hosts = 16 - 2 = 14
Network: 192.168.3.160 | Broadcast: 192.168.3.175
```

**Serial links (/30):**
```
host bits = 32 - 30 = 2
block size = 2^2 = 4
mask = 256 - 4 = 252 → 255.255.255.252
usable hosts = 4 - 2 = 2 (perfect for router-to-router)
```

### Router Configuration

**Router1:**
```
enable
configure terminal
interface fastethernet0/0
ip address 192.168.1.193 255.255.255.192
no shutdown
exit
interface serial2/0
ip address 10.10.0.1 255.255.255.252
clock rate 64000
no shutdown
exit
ip route 192.168.2.0 255.255.255.0 10.10.0.2
ip route 192.168.3.160 255.255.255.240 10.10.0.2
ip route 10.10.1.0 255.255.255.252 10.10.0.2
end
write memory
```

**Router2:**
```
enable
configure terminal
interface serial2/0
ip address 10.10.0.2 255.255.255.252
no shutdown
exit
interface fastethernet0/0
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
interface serial3/0
ip address 10.10.1.1 255.255.255.252
clock rate 64000
no shutdown
exit
ip route 192.168.1.192 255.255.255.192 10.10.0.1
ip route 192.168.3.160 255.255.255.240 10.10.1.2
end
write memory
```

**Router3:**
```
enable
configure terminal
interface serial2/0
ip address 10.10.1.2 255.255.255.252
no shutdown
exit
interface fastethernet0/0
ip address 192.168.3.161 255.255.255.240
no shutdown
exit
ip route 192.168.1.192 255.255.255.192 10.10.1.1
ip route 192.168.2.0 255.255.255.0 10.10.1.1
ip route 10.10.0.0 255.255.255.252 10.10.1.1
end
write memory
```

### Static Routes Logic

Every router must have a route to every network it is NOT directly connected to:

```
5 networks total:
A = 192.168.1.192/26
B = 10.10.0.0/30
C = 192.168.2.0/24
D = 10.10.1.0/30
E = 192.168.3.160/28

Router1 knows: A, B       → needs routes to: C, D, E (all via 10.10.0.2)
Router2 knows: B, C, D    → needs routes to: A (via 10.10.0.1), E (via 10.10.1.2)
Router3 knows: D, E       → needs routes to: A, B, C (all via 10.10.1.1)
```

---

## Knowledge Reference

### OSI Model Summary
| Layer | Name | Devices/Protocols |
|-------|------|-------------------|
| 7 | Application | HTTP, HTTPS, FTP, DNS, DHCP |
| 6 | Presentation | SSL/TLS |
| 5 | Session | Sessions |
| 4 | Transport | TCP, UDP |
| 3 | Network | Router, IP |
| 2 | Data Link | Switch, MAC addresses |
| 1 | Physical | Hub, RJ-45 cables |

### Subnet Mask Quick Reference
| CIDR | Mask | Block Size | Usable Hosts |
|------|------|-----------|--------------|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

### Subnetting Formula
```
host bits  = 32 - prefix
block size = 2^host bits
mask       = 256 - block size (last octet)
hosts      = block size - 2
broadcast  = network address + block size - 1
```

### Protocol Port Reference
| Protocol | Port | Layer | Type |
|----------|------|-------|------|
| FTP Data | 20 | 7 | TCP |
| FTP Control | 21 | 7 | TCP |
| HTTP | 80 | 7 | TCP |
| HTTPS | 443 | 7 | TCP |
| DNS | 53 | 7 | UDP |
| DHCP Server | 67 | 7 | UDP |
| DHCP Client | 68 | 7 | UDP |

### Static Route Command
```
ip route [destination network] [subnet mask] [next hop IP]

To delete a route:
no ip route [destination network] [subnet mask] [next hop IP]

To verify routing table:
show ip route
  C = Connected (directly via interface)
  S = Static    (manually configured)
```

### Cable Type Reference
```
PC    → Switch = Straight-Through
PC    → Hub    = Straight-Through
PC    → Router = Crossover
Switch → Router = Crossover
PC    → PC     = Crossover
Switch → Switch = Crossover
Router → Router = Serial DCE cable
```