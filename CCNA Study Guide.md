# CCNA Study Guide

> A comprehensive study reference for the Cisco Certified Network Associate (CCNA) certification.
>
> **Exam:** 200-301 CCNA — "Implementing and Administering Cisco Solutions"
>
> **Accuracy note:** Exam objectives, question counts, and durations are set by Cisco and change periodically. Every version-specific figure in this guide is attributed to a source, and a [Verification Notes](#verification-notes) section at the end lists what was checked and when. **Always confirm current exam details on Cisco's official page before booking.**

---

## Table of Contents

1. [About the CCNA Certification](#1-about-the-ccna-certification)
2. [Exam Domains & Weightings](#2-exam-domains--weightings)
3. [Study Roadmap](#3-study-roadmap)
4. [Domain 1: Network Fundamentals](#4-domain-1-network-fundamentals-20)
5. [Domain 2: Network Access](#5-domain-2-network-access-20)
6. [Domain 3: IP Connectivity](#6-domain-3-ip-connectivity-25)
7. [Domain 4: IP Services](#7-domain-4-ip-services-10)
8. [Domain 5: Security Fundamentals](#8-domain-5-security-fundamentals-15)
9. [Domain 6: Automation & Programmability](#9-domain-6-automation--programmability-10)
10. [Command Reference](#10-command-reference)
11. [Subnetting Cheat Sheet](#11-subnetting-cheat-sheet)
12. [Lab Environment Setup](#12-lab-environment-setup)
13. [Study Resources](#13-study-resources)
14. [Exam Day Tips](#14-exam-day-tips)
15. [Verification Notes](#verification-notes)

---

# 1. About the CCNA Certification

The **CCNA (Cisco Certified Network Associate)** is an associate-level certification covering network fundamentals, switching/routing, IP services, security fundamentals, and automation.

| Attribute | Detail |
| --- | --- |
| **Exam code** | 200-301 |
| **Exam name** | Implementing and Administering Cisco Solutions |
| **Question types** | Multiple choice, drag-and-drop, fill-in-the-blank, simulation/lab |
| **Validity** | 3 years from pass date |
| **Renewal** | Continuing Education (CE) credits or retake exam |
| **Prerequisites** | None officially stated |
| **Delivery** | Pearson VUE (testing center or online proctored) |

> **Verify before booking:** Question count, duration, and price are set by Cisco and have changed over time. Confirm at <https://learningnetwork.cisco.com/s/ccna> and <https://www.cisco.com/c/en/us/training-events/training-certifications/certifications/associate/ccna.html>.

### What CCNA Does and Doesn't Cover

**Covers:** Routing and switching fundamentals, IPv4/IPv6 addressing, VLANs, STP, EtherChannel, OSPFv2, static routing, NAT, DHCP, DNS, NTP, ACLs, port security, wireless basics, controller-based networking, REST APIs, JSON, Ansible/Puppet/Chef awareness, SDN concepts.

**Does not cover:** Deep BGP, MPLS, advanced QoS, CCIE-level design, extensive cloud networking, or vendor-neutral equivalents in depth. (Some BGP appears as awareness-level content only.)

---

# 2. Exam Domains & Weightings

Per Cisco's published exam blueprint, the six domains and their weightings are:

| Domain | Title | Weight |
| --- | --- | --- |
| 1.0 | Network Fundamentals | 20% |
| 2.0 | Network Access | 20% |
| 3.0 | IP Connectivity | 25% |
| 4.0 | IP Services | 10% |
| 5.0 | Security Fundamentals | 15% |
| 6.0 | Automation and Programmability | 10% |

> **Source:** Cisco's official 200-301 exam topics page: <https://learningnetwork.cisco.com/s/ccna-exam-topics>. Weights have been stable across recent revisions, but confirm on that page before you build a study schedule around them.

---

# 3. Study Roadmap

A practical 10-week plan assuming ~8–10 hours per week. Adjust to your background — experienced network engineers can compress this considerably.

| Week | Focus | Domain Coverage |
| --- | --- | --- |
| 1 | OSI/TCP-IP models, cabling, Ethernet, switching basics | 1.0 |
| 2 | IPv4 addressing, subnetting, IPv6 fundamentals | 1.0 |
| 3 | VLANs, trunking, inter-VLAN routing, STP | 2.0 |
| 4 | EtherChannel, wireless fundamentals, WLC concepts | 2.0 |
| 5 | Static routing, OSPFv2 (single area) | 3.0 |
| 6 | FHRP (HSRP), route summarization, troubleshooting | 3.0 |
| 7 | NAT, DHCP, DNS, NTP, SNMP, syslog | 4.0 |
| 8 | Security fundamentals, ACLs, port security, AAA concepts | 5.0 |
| 9 | Automation, REST APIs, JSON, SDN, Ansible/Puppet/Chef | 6.0 |
| 10 | Full review, practice exams, weak-area drilling | All |

**Study principle:** For every concept, do the lab. CCNA rewards hands-on familiarity — sim questions are unforgiving of memorization-only study.

---

# 4. Domain 1: Network Fundamentals (20%)

## OSI and TCP/IP Models

| OSI Layer | Number | TCP/IP Layer | Protocols/Devices |
| --- | --- | --- | --- |
| Application | 7 | Application | HTTP, FTP, SMTP, DNS, DHCP |
| Presentation | 6 | Application | TLS/SSL, JPEG, ASCII |
| Session | 5 | Application | RPC, NetBIOS |
| Transport | 4 | Transport | TCP, UDP |
| Network | 3 | Internet | IP, ICMP, OSPF, ARP (borderline) |
| Data Link | 2 | Network Access | Ethernet, PPP, MAC, switches |
| Physical | 1 | Network Access | Cables, hubs, signaling |

> **Note:** ARP is often placed at Layer 2 (it deals with MAC resolution) but operates with Layer 3 awareness. Cisco materials generally treat it as a Layer 2/3 boundary protocol. Don't over-index on this — the exam focuses on function, not classification trivia.

## TCP vs UDP

| Feature | TCP | UDP |
| --- | --- | --- |
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Acknowledged, retransmitted | Best-effort |
| Ordering | Sequenced | No ordering guarantee |
| Header size | 20 bytes minimum | 8 bytes |
| Overhead | Higher | Lower |
| Use cases | HTTP(S), SSH, FTP, SMTP | DNS, DHCP, TFTP, VoIP, streaming |

**TCP 3-way handshake:** `SYN` → `SYN-ACK` → `ACK`
**TCP teardown:** `FIN` → `ACK` → `FIN` → `ACK` (four-way)

**Common port numbers:**

| Port | Protocol | Service |
| --- | --- | --- |
| 20/21 | TCP | FTP (data/control) |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 67/68 | UDP | DHCP (server/client) |
| 69 | UDP | TFTP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 123 | UDP | NTP |
| 143 | TCP | IMAP |
| 161/162 | UDP | SNMP (agent/trap) |
| 389 | TCP/UDP | LDAP |
| 443 | TCP | HTTPS |
| 514 | UDP | Syslog |
| 636 | TCP | LDAPS |
| 3306 | TCP | MySQL |
| 3389 | TCP | RDP |

## Ethernet and Switching Basics

**MAC address structure:** 48 bits, written as 12 hex digits (e.g., `00:1A:2B:3C:4D:5E`). First 3 bytes = OUI (vendor), last 3 = NIC-specific.

**Ethernet frame (IEEE 802.3):**

| Field | Size |
| --- | --- |
| Preamble | 7 bytes |
| SFD | 1 byte |
| Destination MAC | 6 bytes |
| Source MAC | 6 bytes |
| Type/Length | 2 bytes |
| Data/Payload | 46–1500 bytes |
| FCS | 4 bytes |

**MTU:** 1500 bytes by default for Ethernet. Jumbo frames extend this (typically 9000) but must be consistent end-to-end.

**Switch operation:**
- Learns source MAC → port mappings into CAM table
- Floods unknown unicast, broadcast, and multicast frames (multicast depends on config)
- Forwards known unicast out the correct port
- Ages out MAC entries (default 300 seconds on Cisco)

**Collision vs broadcast domains:**
- Each switch port = separate collision domain
- All ports in the same VLAN = one broadcast domain
- Routers (or Layer 3 switches) separate broadcast domains

## Cabling and Interfaces

| Cable | Use | Max Distance |
| --- | --- | --- |
| Cat 5e | 1 Gbps Ethernet | 100 m |
| Cat 6 | 1/10 Gbps (10G to 55m) | 100 m (1G) |
| Cat 6a | 10 Gbps | 100 m |
| Fiber (multimode OM3/OM4) | 10/40/100 Gbps | 300–400 m (varies) |
| Fiber (singlemode) | Long-haul | Kilometers |
| Console (rollover) | Out-of-band management | Typically 3–5 m |

**Straight-through vs crossover:** Modern switches support **Auto-MDI/MDIX**, which removes the practical need to choose. Historically: straight-through connected unlike devices (switch↔router), crossover connected like devices (switch↔switch, PC↔PC).

## IPv4 Addressing

**Classes (historical, largely superseded by CIDR):**

| Class | First Octet Range | Default Mask | Purpose |
| --- | --- | --- | --- |
| A | 1–126 | /8 | Large networks |
| B | 128–191 | /16 | Medium networks |
| C | 192–223 | /24 | Small networks |
| D | 224–239 | — | Multicast |
| E | 240–255 | — | Experimental |

**Private ranges (RFC 1918):**

- `10.0.0.0/8`
- `172.16.0.0/12` (172.16.0.0 – 172.31.255.255)
- `192.168.0.0/16`

**Special addresses:**

| Address | Meaning |
| --- | --- |
| `127.0.0.0/8` | Loopback |
| `169.254.0.0/16` | APIPA / link-local (DHCP failure indicator) |
| `0.0.0.0` | "Any"/unspecified (context-dependent) |
| `255.255.255.255` | Limited broadcast |

## IPv6 Fundamentals

**Address format:** 128 bits, eight groups of four hex digits (e.g., `2001:0db8:0000:0000:0000:ff00:0042:8329`).

**Abbreviation rules:**
1. Leading zeros in a group can be dropped: `0db8` → `db8`
2. One contiguous run of all-zero groups can be replaced with `::` (only once per address)

Example: `2001:db8::ff00:42:8329`

**Address types:**

| Type | Prefix | Notes |
| --- | --- | --- |
| Global unicast | `2000::/3` | Routable on the internet |
| Unique local (ULA) | `fc00::/7` | Private equivalent of RFC 1918 |
| Link-local | `fe80::/10` | Auto-configured, not routed |
| Multicast | `ff00::/8` | One-to-many |
| Loopback | `::1/128` | Local host |
| Unspecified | `::/128` | No address assigned |

**Key differences from IPv4:**

- No broadcast (uses multicast and anycast instead)
- No NAT requirement (vast address space)
- No ARP — uses **NDP (Neighbor Discovery Protocol)** via ICMPv6
- SLAAC allows stateless autoconfiguration
- Headers are fixed-length (40 bytes) for faster processing

**EUI-64:** Derives a 64-bit interface identifier from a MAC address by inserting `FF:FE` in the middle and flipping the 7th bit (universal/local bit).

---

# 5. Domain 2: Network Access (20%)

## VLANs

**Purpose:** Logical segmentation of a switch into separate broadcast domains.

**VLAN ranges (Cisco):**

| Range | Name | Notes |
| --- | --- | --- |
| 1 | Default | All ports initially members |
| 2–1001 | Normal | Usable for standard VLANs |
| 1002–1005 | Reserved | Legacy (Token Ring, FDDI) |
| 1006–4094 | Extended | Requires VTP transparent mode |

**Basic VLAN configuration:**

```cisco
! Create VLANs
Switch(config)# vlan 10
Switch(config-vlan)# name Sales
Switch(config-vlan)# exit

Switch(config)# vlan 20
Switch(config-vlan)# name Engineering
Switch(config-vlan)# exit

! Assign access port to VLAN
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10
Switch(config-if)# exit

! Verify
Switch# show vlan brief
Switch# show interfaces switchport
```

## Trunking (802.1Q)

Trunks carry multiple VLANs between switches using tags.

**802.1Q tag:** 4 bytes inserted after the source MAC. Contains a 12-bit VLAN ID (supports 4096 VLANs).

**Native VLAN:** Untagged traffic on a trunk. Default is VLAN 1. **Best practice: change it and match on both ends.** Mismatched native VLANs cause CDP warnings and can create a VLAN-hopping vulnerability.

```cisco
! Configure trunk
Switch(config)# interface gigabitEthernet 0/24
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk native vlan 999
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Switch(config-if)# exit

! Verify
Switch# show interfaces trunk
Switch# show interfaces gigabitEthernet 0/24 switchport
```

> **Note:** `switchport trunk encapsulation dot1q` is required on older platforms that support ISL. On newer Catalyst switches (e.g., 9000 series), 802.1Q is the only option and the command may be absent or unnecessary.

## Inter-VLAN Routing

**Option 1: Router-on-a-Stick** — one physical router interface with subinterfaces per VLAN.

```cisco
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# no shutdown
Router(config-if)# exit

Router(config)# interface gigabitEthernet 0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0
Router(config-subif)# exit

Router(config)# interface gigabitEthernet 0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
Router(config-subif)# exit
```

**Option 2: Layer 3 Switch (SVI)** — preferred in production.

```cisco
Switch(config)# ip routing

Switch(config)# interface vlan 10
Switch(config-if)# ip address 192.168.10.1 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit

Switch(config)# interface vlan 20
Switch(config-if)# ip address 192.168.20.1 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit

! Routed port alternative (point-to-point uplinks)
Switch(config)# interface gigabitEthernet 1/0/1
Switch(config-if)# no switchport
Switch(config-if)# ip address 10.0.0.1 255.255.255.252
```

## Spanning Tree Protocol (STP)

**Purpose:** Prevent Layer 2 loops while providing redundancy.

**802.1D (classic STP) port states:**

| State | Forwards Data? | Learns MACs? | Duration |
| --- | --- | --- | --- |
| Blocking | No | No | 20 sec (max age) |
| Listening | No | No | 15 sec |
| Learning | No | Yes | 15 sec |
| Forwarding | Yes | Yes | — |
| Disabled | No | No | — |

**Convergence time for 802.1D:** ~30–50 seconds.

**Rapid PVST+ (802.1w)** improves this to sub-second for most topologies and is the modern default on Cisco switches.

**Root bridge election:** Lowest bridge ID wins. Bridge ID = priority (default 32768) + MAC address. Lower priority is better; ties broken by lowest MAC.

```cisco
! Force a switch to be root
Switch(config)# spanning-tree vlan 10 root primary
Switch(config)# spanning-tree vlan 20 root secondary

! Or set priority manually (must be a multiple of 4096)
Switch(config)# spanning-tree vlan 10 priority 4096

! Verify
Switch# show spanning-tree
Switch# show spanning-tree vlan 10
Switch# show spanning-tree root
```

**PortFast and BPDU Guard (access ports):**

```cisco
Switch(config)# interface range gigabitEthernet 0/1-20
Switch(config-if-range)# spanning-tree portfast
Switch(config-if-range)# spanning-tree bpduguard enable
Switch(config-if-range)# exit

! Global default (applies to all access ports)
Switch(config)# spanning-tree portfast default
Switch(config)# spanning-tree portfast bpduguard default
```

> **Why it matters:** PortFast skips listening/learning on access ports so endpoints get connectivity immediately. BPDU Guard err-disables the port if a BPDU arrives — protecting against someone plugging in a switch.

## EtherChannel

Bundles multiple physical links into one logical link for bandwidth and redundancy.

**Requirements:** Same speed, duplex, VLAN mode (access/trunk), allowed VLANs, and native VLAN on all member links.

| Protocol | Standard | Notes |
| --- | --- | --- |
| LACP | IEEE 802.3ad | Open standard, `mode active`/`passive` |
| PAgP | Cisco proprietary | `mode desirable`/`auto` |
| Static | None | `mode on` (no negotiation) |

**Mode compatibility:**

| Local | Remote | Result |
| --- | --- | --- |
| active | active | ✅ Works |
| active | passive | ✅ Works |
| passive | passive | ❌ No |
| desirable | desirable | ✅ Works |
| desirable | auto | ✅ Works |
| auto | auto | ❌ No |
| on | on | ✅ Works |
| on | active/passive/desirable/auto | ❌ No |

```cisco
Switch(config)# interface range gigabitEthernet 0/1-2
Switch(config-if-range)# channel-group 1 mode active
Switch(config-if-range)# exit

Switch(config)# interface port-channel 1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20

! Verify
Switch# show etherchannel summary
Switch# show etherchannel port-channel
Switch# show lacp neighbor
```

**Load balancing:** Cisco uses a hash of source/destination MAC, IP, or port (configurable via `port-channel load-balance`). All frames in a single flow traverse the same link.

## Wireless Fundamentals

**Standards (IEEE 802.11):**

| Standard | Band | Max Theoretical Rate | Notes |
| --- | --- | --- | --- |
| 802.11a | 5 GHz | 54 Mbps | Early 5 GHz |
| 802.11b | 2.4 GHz | 11 Mbps | Slow, long range |
| 802.11g | 2.4 GHz | 54 Mbps | Backward-compatible with b |
| 802.11n (Wi-Fi 4) | 2.4/5 GHz | 600 Mbps | MIMO introduced |
| 802.11ac (Wi-Fi 5) | 5 GHz | ~6.9 Gbps | MU-MIMO downlink |
| 802.11ax (Wi-Fi 6/6E) | 2.4/5/6 GHz | ~9.6 Gbps | OFDMA, better density |

> **Rate caveat:** "Max theoretical rate" figures depend on channel width, spatial streams, and MCS. Real throughput is typically 40–60% of the theoretical maximum.

**2.4 GHz channels (non-overlapping):** 1, 6, 11 (North America). Channel 14 is Japan-only.

**Wireless security:**

| Standard | Encryption | Notes |
| --- | --- | --- |
| WEP | RC4 | Broken — do not use |
| WPA | TKIP | Deprecated |
| WPA2 | AES-CCMP | Still widely deployed |
| WPA3 | AES-GCMP / SAE | Current standard; SAE resists offline dictionary attacks |

**WLC deployment models:**

- **Autonomous APs** — individually configured, no controller
- **Lightweight APs + WLC** — centralized management via CAPWAP
- **Cloud-managed** (Meraki) — controller in the cloud
- **Embedded controller** — WLC function on a switch (e.g., Catalyst 9800-CL)

**CAPWAP:** Control traffic uses UDP 5246, data uses UDP 5247.

**AP modes:** Local (default), FlexConnect, Monitor, Sniffer, Bridge, SE-Connect.

---

# 6. Domain 3: IP Connectivity (25%)

## Routing Fundamentals

**Administrative Distance (AD)** — trustworthiness of a route source. Lower is better.

| Route Source | Default AD |
| --- | --- |
| Connected interface | 0 |
| Static route | 1 |
| EIGRP (internal) | 90 |
| OSPF | 110 |
| RIP | 120 |
| EIGRP (external) | 170 |
| Unknown / unreachable | 255 |

**Metric:** Tie-breaker within the same protocol. OSPF uses cost (based on bandwidth); EIGRP uses a composite metric.

**Routing table lookup order:**
1. Longest prefix match (most specific route wins)
2. Administrative distance
3. Metric

## Static Routing

```cisco
! Basic static route
Router(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2

! Default route
Router(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1

! Floating static (higher AD as backup)
Router(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2 200

! Exit-interface only (not recommended on Ethernet — causes proxy ARP per destination)
Router(config)# ip route 192.168.20.0 255.255.255.0 gigabitEthernet 0/1

! IPv6 static route
Router(config)# ipv6 route 2001:db8:20::/64 2001:db8:1::2

! Verify
Router# show ip route
Router# show ip route static
Router# show ip route 192.168.20.1
```

**Route codes in `show ip route`:**

| Code | Meaning |
| --- | --- |
| `C` | Connected |
| `L` | Local (the interface's own IP) |
| `S` | Static |
| `S*` | Static default |
| `O` | OSPF |
| `D` | EIGRP |
| `R` | RIP |
| `*` | Candidate default |

## OSPFv2 (Single Area)

**Basics:** Link-state protocol, uses Dijkstra's SPF algorithm, converges quickly, supports VLSM/CIDR.

**Key concepts:**

- **Area 0** (backbone) — all other areas must connect to it
- **Router ID** — highest loopback IP, then highest physical interface IP (unless manually set)
- **Cost** = reference bandwidth ÷ interface bandwidth (default reference = 100 Mbps, so a 1 Gbps link costs 1 — this is why the reference bandwidth is often raised)
- **Neighbor states:** Down → Init → 2-Way → ExStart → Exchange → Loading → Full
- **Hello timer:** 10 sec (broadcast), 30 sec (NBMA); Dead timer = 4× hello

**Requirements for adjacency:**
- Matching area ID
- Matching hello/dead timers
- Matching authentication
- Matching MTU (on the same subnet)
- Unique router IDs
- Matching network type (mostly)

**Basic configuration:**

```cisco
Router(config)# router ospf 1
Router(config-router)# router-id 1.1.1.1
Router(config-router)# network 10.0.0.0 0.0.0.255 area 0
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router(config-router)# exit

! Interface-level alternative (modern approach)
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip ospf 1 area 0
Router(config-if)# ip ospf cost 10

! Raise reference bandwidth (important on modern links)
Router(config-router)# auto-cost reference-bandwidth 10000

! Verify
Router# show ip ospf neighbor
Router# show ip ospf interface brief
Router# show ip route ospf
Router# show ip protocols
```

**Passive interfaces (best practice — stops hellos on edge ports):**

```cisco
Router(config-router)# passive-interface default
Router(config-router)# no passive-interface gigabitEthernet 0/0
```

## First Hop Redundancy Protocols (FHRP)

Provides a virtual gateway IP shared between routers.

| Protocol | Standard | Notes |
| --- | --- | --- |
| HSRP | Cisco proprietary | Active/Standby; virtual MAC `0000.0c07.acXX` |
| VRRP | IEEE standard | Master/Backup; virtual MAC `0000.5e00.01XX` |
| GLBP | Cisco proprietary | Active/Active load balancing |

**HSRP configuration:**

```cisco
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# standby 1 ip 192.168.10.1
Router(config-if)# standby 1 priority 150
Router(config-if)# standby 1 preempt
Router(config-if)# standby 1 track gigabitEthernet 0/1 30
Router(config-if)# exit

Router# show standby brief
```

**HSRP states:** Initial → Learn → Listen → Speak → Standby → Active

Default priority is 100. Higher wins. **Preemption is off by default** — without `preempt`, a recovered higher-priority router won't reclaim the active role.

---

# 7. Domain 4: IP Services (10%)

## NAT (Network Address Translation)

| Type | Description |
| --- | --- |
| Static NAT | One-to-one permanent mapping |
| Dynamic NAT | Pool-based, one-to-one per session |
| PAT (NAT overload) | Many-to-one using port numbers |

**Terminology:**
- **Inside local** — private IP of internal host
- **Inside global** — public IP as seen externally
- **Outside local** — external host's IP as seen internally
- **Outside global** — external host's real IP

**Static NAT:**

```cisco
Router(config)# ip nat inside source static 192.168.1.10 203.0.113.10

Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside

Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
```

**PAT (overload):**

```cisco
! Using an interface IP
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload

! Using a pool
Router(config)# ip nat pool PUBLIC 203.0.113.10 203.0.113.20 netmask 255.255.255.240
Router(config)# ip nat inside source list 1 pool PUBLIC overload

Router(config)# access-list 1 permit 192.168.0.0 0.0.255.255

! Verify
Router# show ip nat translations
Router# show ip nat statistics
Router# clear ip nat translation *
```

## DHCP

**DORA process:** Discover → Offer → Request → Acknowledge

**DHCP server configuration:**

```cisco
Router(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10

Router(config)# ip dhcp pool LAN10
Router(dhcp-config)# network 192.168.10.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.10.1
Router(dhcp-config)# dns-server 8.8.8.8 8.8.4.4
Router(dhcp-config)# lease 7
Router(dhcp-config)# exit

! Verify
Router# show ip dhcp binding
Router# show ip dhcp pool
Router# show ip dhcp conflict
```

**DHCP relay (when server is on another subnet):**

```cisco
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip helper-address 10.0.0.50
```

> **Note:** `ip helper-address` forwards several UDP broadcasts by default, not just DHCP (TFTP, DNS, NetBIOS, etc.). Use `no ip forward-protocol udp <port>` to restrict if needed.

## DNS

```cisco
! Enable DNS resolution (client-side)
Router(config)# ip domain-lookup

! Specify name servers
Router(config)# ip name-server 8.8.8.8 1.1.1.1

! Static host entries
Router(config)# ip host server1 192.168.1.50

! Local DNS server (rarely used on IOS)
Router(config)# ip dns server

! Verify
Router# show hosts
Router# show ip dns view
```

## NTP (Network Time Protocol)

```cisco
! Configure as NTP client
Router(config)# ntp server 216.239.35.0 prefer
Router(config)# ntp server 216.239.35.4

! Set timezone
Router(config)# clock timezone UTC 0

! Authentication (recommended)
Router(config)# ntp authentication-key 1 md5 SecretKey123
Router(config)# ntp authenticate
Router(config)# ntp trusted-key 1
Router(config)# ntp server 216.239.35.0 key 1

! Verify
Router# show ntp status
Router# show ntp associations
```

**Stratum levels:** Stratum 0 = atomic clocks; Stratum 1 = directly attached; each subsequent level increments. Cisco routers default to Stratum 16 (unsynchronized).

## SNMP

| Version | Security |
| --- | --- |
| v1 | Community string (cleartext) |
| v2c | Community string (cleartext) |
| v3 | Authentication + encryption (recommended) |

```cisco
! SNMPv2c (lab only)
Router(config)# snmp-server community PublicRO ro
Router(config)# snmp-server community PrivateRW rw

! SNMPv3 (production)
Router(config)# snmp-server group ADMIN v3 priv
Router(config)# snmp-server user netadmin ADMIN v3 auth sha AuthPass123 priv aes 128 PrivPass123
Router(config)# snmp-server host 10.0.0.100 version 3 priv netadmin

! Verify
Router# show snmp
Router# show snmp user
Router# show snmp group
```

## Syslog

**Severity levels (0 = most severe):**

| Level | Name | Meaning |
| --- | --- | --- |
| 0 | Emergency | System unusable |
| 1 | Alert | Immediate action needed |
| 2 | Critical | Critical condition |
| 3 | Error | Error condition |
| 4 | Warning | Warning condition |
| 5 | Notice | Normal but significant |
| 6 | Informational | Informational |
| 7 | Debugging | Debug output |

```cisco
! Send logs to a syslog server
Router(config)# logging host 10.0.0.100
Router(config)# logging trap informational

! Local buffering
Router(config)# logging buffered 16384 informational
Router(config)# logging console warnings

! Timestamps (essential for correlation)
Router(config)# service timestamps log datetime msec localtime show-timezone

! Verify
Router# show logging
```

## QoS Awareness

CCNA covers QoS conceptually rather than in depth.

| Traffic Characteristic | Recommended Treatment |
| --- | --- |
| Voice (RTP) | Priority queuing (LLQ), low latency/jitter |
| Video | Guaranteed bandwidth |
| Interactive data | Fair queuing |
| Bulk/backup | Lowest priority, scavenger class |

**Key mechanisms:** Classification and marking (DSCP/CoS), congestion management (queuing), congestion avoidance (WRED), policing vs shaping.

**Common DSCP values:**

| DSCP | Per-hop behavior | Typical use |
| --- | --- | --- |
| EF (46) | Expedited Forwarding | Voice |
| AF41 (34) | Assured Forwarding | Video |
| AF31 (26) | Assured Forwarding | Streaming |
| CS1 (8) | Class Selector 1 | Scavenger |
| DF (0) | Default | Best effort |

---

# 8. Domain 5: Security Fundamentals (15%)

## Security Concepts

**CIA triad:** Confidentiality, Integrity, Availability.

**Common threats:**

| Threat | Description |
| --- | --- |
| DoS/DDoS | Resource exhaustion |
| Spoofing | Faking identity (IP, MAC, DNS) |
| MITM | Intercepting communications |
| Phishing | Social engineering |
| VLAN hopping | Double tagging or switch spoofing |
| MAC flooding | CAM table overflow |
| ARP poisoning | Gratuitous ARP abuse |

**Defense in depth:** Layered controls — physical, network, host, application, data.

## ACLs (Access Control Lists)

**Standard ACLs** — filter by source IP only. Place close to the destination.

```cisco
Router(config)# access-list 10 permit 192.168.1.0 0.0.0.255
Router(config)# access-list 10 deny any

Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip access-group 10 out
```

**Extended ACLs** — filter by source, destination, protocol, port. Place close to the source.

```cisco
Router(config)# ip access-list extended WEB-FILTER
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 80
Router(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 any eq 443
Router(config-ext-nacl)# deny ip any any log
Router(config-ext-nacl)# exit

Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip access-group WEB-FILTER in
```

**Wildcard masks:** Inverse of subnet masks. `0` = must match, `1` = ignore.

| Wildcard | Matches |
| --- | --- |
| `0.0.0.0` | Exactly one host |
| `0.0.0.255` | A /24 |
| `0.0.255.255` | A /16 |
| `0.0.0.63` | A /26 |
| `any` | All addresses |

**Named ACL editing:**

```cisco
Router(config)# ip access-list extended MY-ACL
Router(config-ext-nacl)# 10 permit tcp any any eq 22
Router(config-ext-nacl)# 20 deny ip any any
Router(config-ext-nacl)# no 20
Router(config-ext-nacl)# 30 permit ip any any
```

> **Critical:** Every ACL has an implicit `deny any` at the end. Always check what you're blocking before applying.

## Port Security

```cisco
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport port-security
Switch(config-if)# switchport port-security maximum 2
Switch(config-if)# switchport port-security mac-address sticky
Switch(config-if)# switchport port-security violation restrict
Switch(config-if)# exit

! Verify
Switch# show port-security
Switch# show port-security interface gigabitEthernet 0/1
Switch# show port-security address
```

**Violation modes:**

| Mode | Action | Counter Increment |
| --- | --- | --- |
| Protect | Drop offending frames | No |
| Restrict | Drop + log + SNMP trap | Yes |
| Shutdown | Err-disable the port | Yes |

**Recovering an err-disabled port:**

```cisco
Switch(config)# interface gigabitEthernet 0/1
Switch(config-if)# shutdown
Switch(config-if)# no shutdown
```

Or configure auto-recovery:

```cisco
Switch(config)# errdisable recovery cause psecure-violation
Switch(config)# errdisable recovery interval 300
```

## DHCP Snooping

Prevents rogue DHCP servers by trusting only designated ports.

```cisco
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10,20
Switch(config)# no ip dhcp snooping information option

Switch(config)# interface gigabitEthernet 0/24
Switch(config-if)# ip dhcp snooping trust

! Rate limit untrusted ports
Switch(config)# interface range gigabitEthernet 0/1-20
Switch(config-if-range)# ip dhcp snooping limit rate 10

! Verify
Switch# show ip dhcp snooping
Switch# show ip dhcp snooping binding
```

## Dynamic ARP Inspection (DAI)

Validates ARP packets against the DHCP snooping binding table.

```cisco
Switch(config)# ip arp inspection vlan 10,20

Switch(config)# interface gigabitEthernet 0/24
Switch(config-if)# ip arp inspection trust

! Verify
Switch# show ip arp inspection
Switch# show ip arp inspection statistics
```

## AAA Concepts

| Component | Purpose | Examples |
| --- | --- | --- |
| Authentication | Who are you? | Local, RADIUS, TACACS+ |
| Authorization | What can you do? | Privilege levels, command sets |
| Accounting | What did you do? | Logging, billing |

**RADIUS vs TACACS+:**

| Feature | RADIUS | TACACS+ |
| --- | --- | --- |
| Transport | UDP 1812/1813 | TCP 49 |
| Encryption | Password only | Entire payload |
| Vendor | Open standard | Cisco proprietary |
| AAA separation | Combined | Separated |
| Typical use | Network access | Device administration |

**Local AAA configuration:**

```cisco
Router(config)# aaa new-model
Router(config)# aaa authentication login default local
Router(config)# aaa authorization exec default local

Router(config)# username admin privilege 15 secret StrongPassword123
Router(config)# enable secret EnablePassword456

! Local fallback
Router(config)# aaa authentication login default group tacacs+ local
```

## Device Hardening

```cisco
! Secure privileged EXEC
Router(config)# enable secret StrongSecret

! Encrypt plaintext passwords in config
Router(config)# service password-encryption

! Console and VTY hardening
Router(config)# line console 0
Router(config-line)# password ConsolePass
Router(config-line)# login
Router(config-line)# exec-timeout 5 0
Router(config-line)# logging synchronous
Router(config-line)# exit

Router(config)# line vty 0 4
Router(config-line)# transport input ssh
Router(config-line)# exec-timeout 5 0
Router(config-line)# login local
Router(config-line)# exit

! SSH configuration
Router(config)# ip domain-name example.com
Router(config)# crypto key generate rsa modulus 2048
Router(config)# ip ssh version 2
Router(config)# ip ssh time-out 60
Router(config)# ip ssh authentication-retries 3

! Disable unused services
Router(config)# no ip http server
Router(config)# no cdp run
Router(config)# no ip source-route
Router(config)# no ip finger

! Banner
Router(config)# banner motd ^
Unauthorized access prohibited. All activity is monitored.
^
```

> **Note:** `service password-encryption` uses a weak, reversible Type 7 encoding. It obscures casual shoulder-surfing but is **not** cryptographically secure. Use `enable secret` (Type 5 MD5 or Type 8/9 scrypt/PBKDF2) instead of `enable password`.

---

# 9. Domain 6: Automation & Programmability (10%)

## SDN and Controller-Based Networking

**Traditional networking:** Distributed control plane — each device makes independent decisions.

**SDN architecture:**

| Plane | Function |
| --- | --- |
| Data plane | Forwards packets (hardware) |
| Control plane | Makes routing/switching decisions |
| Management plane | Configuration, monitoring (SSH, SNMP, APIs) |

**Cisco SDN offerings:**

- **Cisco DNA Center** — campus fabric management
- **Cisco SD-WAN (vManage)** — WAN orchestration
- **Cisco ACI** — data center fabric
- **Meraki Dashboard** — cloud-managed networking

**Southbound vs Northbound APIs:**

- **Southbound** — controller → devices (NETCONF, RESTCONF, OpenFlow, CLI/SNMP)
- **Northbound** — applications → controller (REST APIs)

## REST APIs

**HTTP methods:**

| Method | Purpose | Idempotent? |
| --- | --- | --- |
| GET | Retrieve | Yes |
| POST | Create | No |
| PUT | Replace | Yes |
| PATCH | Partial update | No |
| DELETE | Remove | Yes |

**Status codes:**

| Code | Meaning |
| --- | --- |
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

**Example: Cisco DNA Center API call**

```bash
# Authenticate
curl -X POST https://dnac.example.com/dna/system/api/v1/auth/token \
  -H "Authorization: Basic $(echo -n 'admin:password' | base64)" \
  -H "Content-Type: application/json"

# Use the token
curl -X GET https://dnac.example.com/dna/intent/api/v1/network-device \
  -H "X-Auth-Token: ${TOKEN}" \
  -H "Content-Type: application/json"
```

## Data Formats

**JSON (JavaScript Object Notation):**

```json
{
  "device": {
    "hostname": "core-sw-01",
    "ip": "10.0.0.1",
    "interfaces": [
      { "name": "Gi0/1", "status": "up" },
      { "name": "Gi0/2", "status": "down" }
    ]
  }
}
```

**Rules:**
- Keys must be double-quoted strings
- Values: string, number, boolean, null, object, array
- No trailing commas
- No comments

**YAML (used by Ansible):**

```yaml
---
- name: Configure VLANs
  hosts: switches
  gather_facts: no
  tasks:
    - name: Create VLAN 10
      ios_vlan:
        vlan_id: 10
        name: Sales
        state: present
```

**XML (used by NETCONF):**

```xml
<config>
  <interfaces>
    <interface>
      <name>GigabitEthernet0/1</name>
      <enabled>true</enabled>
    </interface>
  </interfaces>
</config>
```

## Configuration Management Tools

| Tool | Language | Model | Agent Required? |
| --- | --- | --- | --- |
| Ansible | Python (YAML playbooks) | Push | No (agentless, SSH) |
| Puppet | Ruby (DSL manifests) | Pull | Yes |
| Chef | Ruby (recipes/cookbooks) | Pull | Yes |
| SaltStack | Python (states) | Push/Pull | Optional |

**Ansible advantages:** Agentless, uses SSH, simple YAML syntax, large module library including `ios_*`, `nxos_*`, `eos_*` collections.

**Example Ansible inventory:**

```ini
[switches]
sw1 ansible_host=10.0.0.11
sw2 ansible_host=10.0.0.12

[switches:vars]
ansible_user=admin
ansible_password=SecurePass
ansible_network_os=ios
ansible_connection=network_cli
```

## NETCONF and RESTCONF

| Feature | NETCONF | RESTCONF |
| --- | --- | --- |
| Transport | SSH (TCP 830) | HTTPS (TCP 443) |
| Encoding | XML | XML or JSON |
| Model | YANG | YANG |
| Style | RPC-based | REST-based |

**YANG:** Data modeling language defining the structure of configuration and state data. Models include native (vendor-specific) and IETF/OpenConfig (standardized).

---

# 10. Command Reference

## Device Management

```cisco
! Basic setup
enable
configure terminal
hostname R1
no ip domain-lookup

! Save configuration
copy running-config startup-config
write memory

! View configuration
show running-config
show startup-config

! Erase configuration
erase startup-config
reload
```

## Interface Configuration

```cisco
interface gigabitEthernet 0/0
 description Uplink to ISP
 ip address 203.0.113.2 255.255.255.252
 no shutdown
 exit

! Verify
show ip interface brief
show interfaces gigabitEthernet 0/0
show interfaces status
show interfaces counters errors
```

## Verification Commands (Most-Tested)

```cisco
! Layer 2
show vlan brief
show interfaces trunk
show spanning-tree
show spanning-tree summary
show etherchannel summary
show mac address-table
show cdp neighbors
show cdp neighbors detail
show lldp neighbors

! Layer 3
show ip route
show ip route ospf
show ip protocols
show ip ospf neighbor
show ip ospf interface brief
show ip interface brief
show arp
show ip arp

! Services
show ip dhcp binding
show ip nat translations
show ip nat statistics
show ntp status
show logging
show snmp

! Security
show access-lists
show ip access-lists
show port-security
show ip dhcp snooping
show ip arp inspection

! Troubleshooting
ping 8.8.8.8
ping 8.8.8.8 source gigabitEthernet 0/0
traceroute 8.8.8.8
debug ip icmp
undebug all
terminal monitor
```

## Configuration Modes

| Prompt | Mode | Enter With |
| --- | --- | --- |
| `Router>` | User EXEC | Login |
| `Router#` | Privileged EXEC | `enable` |
| `Router(config)#` | Global config | `configure terminal` |
| `Router(config-if)#` | Interface config | `interface gi0/0` |
| `Router(config-router)#` | Routing protocol config | `router ospf 1` |
| `Router(config-line)#` | Line config | `line vty 0 4` |
| `Router(config-vlan)#` | VLAN config | `vlan 10` |
| `Router(config-ext-nacl)#` | Named ACL config | `ip access-list extended NAME` |

**Navigation shortcuts:**

| Command | Effect |
| --- | --- |
| `exit` | Back one level |
| `end` | Return to privileged EXEC |
| `Ctrl+Z` | Same as `end` |
| `do <command>` | Run EXEC command from config mode |
| `Ctrl+A` | Move to start of line |
| `Ctrl+E` | Move to end of line |
| `Ctrl+Shift+6` | Break (abort ping/traceroute) |

---

# 11. Subnetting Cheat Sheet

## Powers of Two

| Power | Value | Power | Value |
| --- | --- | --- | --- |
| 2¹ | 2 | 2⁶ | 64 |
| 2² | 4 | 2⁷ | 128 |
| 2³ | 8 | 2⁸ | 256 |
| 2⁴ | 16 | 2⁹ | 512 |
| 2⁵ | 32 | 2¹⁰ | 1024 |

## CIDR Reference Table

| CIDR | Subnet Mask | Wildcard | Hosts | Block Size |
| --- | --- | --- | --- | --- |
| /30 | 255.255.255.252 | 0.0.0.3 | 2 | 4 |
| /29 | 255.255.255.248 | 0.0.0.7 | 6 | 8 |
| /28 | 255.255.255.240 | 0.0.0.15 | 14 | 16 |
| /27 | 255.255.255.224 | 0.0.0.31 | 30 | 32 |
| /26 | 255.255.255.192 | 0.0.0.63 | 62 | 64 |
| /25 | 255.255.255.128 | 0.0.0.127 | 126 | 128 |
| /24 | 255.255.255.0 | 0.0.0.255 | 254 | 256 |
| /23 | 255.255.254.0 | 0.0.1.255 | 510 | 512 |
| /22 | 255.255.252.0 | 0.0.3.255 | 1022 | 1024 |
| /21 | 255.255.248.0 | 0.0.7.255 | 2046 | 2048 |
| /20 | 255.255.240.0 | 0.0.15.255 | 4094 | 4096 |
| /19 | 255.255.224.0 | 0.0.31.255 | 8190 | 8192 |
| /18 | 255.255.192.0 | 0.0.63.255 | 16382 | 16384 |
| /17 | 255.255.128.0 | 0.0.127.255 | 32766 | 32768 |
| /16 | 255.255.0.0 | 0.0.255.255 | 65534 | 65536 |
| /12 | 255.240.0.0 | 0.15.255.255 | 1048574 | 1048576 |
| /8 | 255.0.0.0 | 0.255.255.255 | 16777214 | 16777216 |

> **Formula:** Hosts = 2^(32 − prefix) − 2 (subtracting network and broadcast addresses). Block size = 2^(32 − prefix) in the interesting octet.

## Subnetting Method (Fast Approach)

**Example: 192.168.1.130/26**

1. **Prefix → block size:** /26 → 2^(32−26) = 64
2. **Find the subnet:** multiples of 64 → 0, 64, 128, 192. So the subnet is `192.168.1.128`
3. **Broadcast:** next subnet minus 1 → `192.168.1.191`
4. **Usable range:** `192.168.1.129` – `192.168.1.190`
5. **Mask:** `255.255.255.192`
6. **Wildcard:** `0.0.0.63`

## VLSM Example

Given `192.168.1.0/24`, allocate for:

| Requirement | Hosts Needed | Prefix | Allocation |
| --- | --- | --- | --- |
| Sales | 100 | /25 | 192.168.1.0/25 (126 usable) |
| Engineering | 50 | /26 | 192.168.1.128/26 (62 usable) |
| Servers | 20 | /27 | 192.168.1.192/27 (30 usable) |
| WAN links | 2 each | /30 | 192.168.1.224/30, .228/30, .232/30 |

**Rule:** Allocate largest subnets first to avoid fragmentation.

## IPv6 Subnetting

IPv6 uses a /64 for almost all LAN segments (SLAAC requires it).

```
2001:db8:acad:0001::/64  → LAN 1
2001:db8:acad:0002::/64  → LAN 2
2001:db8:acad:0003::/64  → LAN 3
```

The first 48 bits are typically the global routing prefix (assigned by ISP), the next 16 bits are the subnet ID, and the last 64 bits are the interface ID.

---

# 12. Lab Environment Setup

## Option 1: Cisco Packet Tracer (Free)

**Pros:** Free with Cisco Networking Academy enrollment, lightweight, includes simulation mode.
**Cons:** Simplified IOS, limited feature set, some commands behave differently from real hardware.

Download: <https://www.netacad.com/courses/packet-tracer>

## Option 2: GNS3 (Free)

**Pros:** Runs real IOS images, supports complex topologies, integrates with VMs.
**Cons:** Requires IOS images (licensing considerations), resource-intensive.

```bash
# Install on Ubuntu/Debian
sudo add-apt-repository ppa:gns3/ppa
sudo apt update
sudo apt install gns3-gui gns3-server

# Verify
gns3server --version
```

> **Licensing note:** Cisco IOS images are licensed software. Only use images you're entitled to (e.g., via Cisco VIRL/CML subscription or your employer's license).

## Option 3: Cisco Modeling Labs (CML)

**Pros:** Official Cisco virtual lab, legal IOS images, cloud or on-premises.
**Cons:** Paid subscription.

## Option 4: EVE-NG

**Pros:** Powerful, web-based, supports many vendor images.
**Cons:** Community edition has limitations; images again a licensing consideration.

## Recommended Lab Topologies

**Lab 1: Basic switching**

```text
[PC1]---[SW1]---[SW2]---[PC2]
         |         |
      (VLAN 10)  (VLAN 20)
```

Practice: VLAN creation, trunking, inter-VLAN routing, STP.

**Lab 2: Routing**

```text
[R1]---[R2]---[R3]
  |             |
[LAN A]      [LAN B]
```

Practice: static routes, OSPF, route redistribution, floating statics.

**Lab 3: Services and security**

```text
[LAN]---[R1]---[Internet]
         |
    [DHCP/DNS/NTP server]
```

Practice: NAT, DHCP, ACLs, SSH, port security.

---

# 13. Study Resources

## Official Cisco Resources

| Resource | Link |
| --- | --- |
| CCNA exam page | <https://learningnetwork.cisco.com/s/ccna> |
| Official exam topics | <https://learningnetwork.cisco.com/s/ccna-exam-topics> |
| Cisco Networking Academy | <https://www.netacad.com/> |
| Cisco DevNet (automation) | <https://developer.cisco.com/> |

## Books

| Title | Author(s) | Notes |
| --- | --- | --- |
| CCNA 200-301 Official Cert Guide, Vol 1 & 2 | Wendell Odom | The canonical reference |
| CCNA 200-301 Official Cert Guide Library | Wendell Odom | Boxed set of both volumes |
| 31 Days Before Your CCNA Exam | Allan Johnson | Final-review pacing guide |

> **Edition caution:** Book editions are revised as Cisco updates the exam. Check that you're buying the current edition for 200-301.

## Video Courses

- **Jeremy's IT Lab** (YouTube) — free, comprehensive, highly regarded
- **David Bombal** — practical labs and packet-level analysis
- **Keith Barker** — CBT Nuggets, conceptual depth
- **Boson ExSim-Max** — practice exams widely considered the closest to the real thing

## Practice Exams

| Provider | Notes |
| --- | --- |
| Boson ExSim-Max | Paid; hardest and most realistic |
| Pearson Test Prep | Official practice questions |
| Cisco Learning Network | Free sample questions |

> **Warning about brain dumps:** Using actual exam question dumps violates Cisco's certification agreement and can result in revocation. Stick to legitimate practice exams.

## Communities

- r/ccna (Reddit)
- Cisco Learning Network forums
- Discord study groups (search for CCNA study servers)

---

# 14. Exam Day Tips

## Before the Exam

1. **Confirm logistics** — testing center location or online proctoring requirements
2. **Bring valid ID** — government-issued photo ID matching your registration name exactly
3. **Arrive early** — 15–30 minutes for check-in
4. **Review your weak areas** — not new material, just reinforcement
5. **Sleep** — cognitive performance matters more than last-minute cramming

## During the Exam

**Time management:**
- Simulation/lab questions take the longest — don't get stuck
- If a sim is taking more than ~8 minutes, note it and move on
- Answer every question (no penalty for guessing)

**Simulation strategy:**
1. Read the entire task before typing anything
2. Use `?` for command help
3. Verify with `show` commands before moving on
4. Remember: `copy running-config startup-config` if the task implies persistence
5. Check that interfaces are `no shutdown`

**Common sim pitfalls:**
- Forgetting to configure the native VLAN on both ends of a trunk
- Applying an ACL in the wrong direction
- Missing `ip routing` on a Layer 3 switch
- Wrong wildcard mask in ACLs
- Forgetting `no shutdown` on SVIs

**Multiple choice strategy:**
- Eliminate obviously wrong answers first
- Watch for "choose two" / "choose three" — partial credit usually isn't given
- Beware of absolute words ("always", "never") in distractors
- If two answers seem right, pick the one that's more specific to the scenario

## After the Exam

- **Pass:** Certificate available via Cisco Certification Tracking System; CE credits begin accruing
- **Fail:** Cisco provides a domain-level breakdown; focus your retake on the weakest domains
- **Retake policy:** Cisco enforces a waiting period between attempts (check current policy)

---

# Verification Notes

This guide was written with attention to accuracy. Below is a summary of what was verified, what is version-dependent, and what readers should confirm independently.

## Verified Against Official Sources

| Item | Source |
| --- | --- |
| Exam code (200-301) and name | Cisco Learning Network |
| Six domains and their weightings (20/20/25/10/15/10) | Cisco official exam topics page |
| Exam delivery via Pearson VUE | Cisco certification page |

## Version-Dependent — Confirm Before Booking

| Item | Why It Varies |
| --- | --- |
| Exact question count | Cisco does not publicly commit to a fixed number; historically ~100 questions |
| Exam duration | Has varied across revisions |
| Exam price | Varies by country and currency |
| Retake waiting period | Policy has changed over time |
| Passing score | Cisco does not publish a fixed passing score; it's scaled per exam form |

> **Action:** Before scheduling, check <https://learningnetwork.cisco.com/s/ccna> for current specifics.

## Technical Content Notes

**Deliberately generalized:**

- **OSI layer placement of ARP** — sources disagree (Layer 2 vs Layer 2/3 boundary). The guide notes this rather than asserting one answer.
- **Wireless "max theoretical rate"** — figures depend on channel width, spatial streams, and MCS index. Real-world throughput is substantially lower. The table is for relative comparison, not absolute specification.
- **Ethernet frame fields** — the guide shows the IEEE 802.3 frame. DIX Ethernet II differs slightly (no length field; uses EtherType). Modern networks use Ethernet II framing predominantly.
- **Cat 6 10GBASE-T distance** — 55 m is the commonly cited figure for Cat 6 at 10 Gbps; Cat 6a reaches 100 m.

**Command notes:**

- `switchport trunk encapsulation dot1q` is platform-dependent. It's required on older Catalyst switches supporting both ISL and 802.1Q; newer platforms (Catalyst 9000) only support 802.1Q and the command may be absent.
- `service password-encryption` uses reversible Type 7 encoding — explicitly flagged as not cryptographically secure.
- `ip helper-address` forwards multiple UDP broadcast types by default, not only DHCP — noted with the `no ip forward-protocol` mitigation.

**Hardware/platform caveats:**

- HSRP virtual MAC format (`0000.0c07.acXX`) is standard; VRRP uses `0000.5e00.01XX`.
- EtherChannel mode compatibility table reflects standard Cisco behavior.
- Port security violation mode behaviors (protect/restrict/shutdown) are per Cisco documentation.

## Things This Guide Does Not Claim

- It does not reproduce actual exam questions (that would violate Cisco's certification agreement).
- It does not guarantee a passing score.
- It does not replace the official exam topics document — always cross-check against Cisco's published blueprint.

## Maintenance

If you fork or contribute to this guide:

1. Verify any new exam-specific figures against Cisco's official pages before committing
2. Note the date of verification
3. Flag platform-dependent commands rather than presenting them as universal
4. Do not add brain-dump content

---

**Last Updated:** October 2026
**License:** Educational use. Cisco, CCNA, and related marks are trademarks of Cisco Systems, Inc. This guide is unofficial and not endorsed by Cisco.
