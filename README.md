# Enterprise Dual-WAN OSPF Network Lab

A multi-site enterprise network designed and implemented in Cisco Packet Tracer 9.0.1.0.

This project demonstrates enterprise networking concepts including VLANs, router-on-a-stick, IPv4 subnetting, dual-WAN architecture, OSPF dynamic routing, WAN aggregation, and network redundancy.

---

## 📌 Project Overview

The network represents a small enterprise with:

- 1 Headquarters (HQ)
- 3 Branch Offices
- 2 WAN/Core routers
- 4 site routers
- 6 Cisco switches
- 8 end-user PCs
- Dual WAN paths
- OSPF Area 0

The design provides dynamic routing between all sites while maintaining two independent WAN paths.

---

## 🏗️ Network Architecture


                         WAN-1
                           |
                          R5
                     WAN/Core Router
                    /    |    |    \
                   /     |    |     \
                 R1      R2   R3      R4
                 HQ    Branch1 Branch2 Branch3
                  \      |      |      /
                   \     |      |     /
                          R6
                     WAN/Core Router
                           |
                         WAN-2
```

Each site router has three connections:


G0/0 → Local LAN
G0/1 → WAN-1
G0/2 → WAN-2
```

---

## 🖥️ Devices

| Device | Role |
|---|---|
| R1 | HQ Router |
| R2 | Branch 1 Router |
| R3 | Branch 2 Router |
| R4 | Branch 3 Router |
| R5 | WAN-1/Core Router |
| R6 | WAN-2/Core Router |
| SW1 | HQ LAN Switch |
| SW2 | Branch 1 LAN Switch |
| SW3 | Branch 2 LAN Switch |
| SW4 | Branch 3 LAN Switch |
| SW5 | WAN-1 Aggregation Switch |
| SW6 | WAN-2 Aggregation Switch |
| PC1-PC8 | End-user devices |

---

## 🌐 IP Addressing

### Site LANs

| Site      | Network      | Default Gateway |

|    HQ    | 10.10.10.0/24 | 10.10.10.1 |
| Branch 1 | 10.20.20.0/24 | 10.20.20.1 |
| Branch 2 | 10.30.30.0/24 | 10.30.30.1 |
| Branch 3 | 10.40.40.0/24 | 10.40.40.1 |

### WAN-1

| Connection |   Network  | IPs |

| R5 ↔ R1 | 172.16.1.0/30 | R5: .1 / R1: .2 |
| R5 ↔ R2 | 172.16.2.0/30 | R5: .1 / R2: .2 |
| R5 ↔ R3 | 172.16.3.0/30 | R5: .1 / R3: .2 |
| R5 ↔ R4 | 172.16.4.0/30 | R5: .1 / R4: .2 |

### WAN-2

| Connection | Network     |    IPs |

| R6 ↔ R1 | 172.16.11.0/30 | R6: .1 / R1: .2 |
| R6 ↔ R2 | 172.16.12.0/30 | R6: .1 / R2: .2 |
| R6 ↔ R3 | 172.16.13.0/30 | R6: .1 / R3: .2 |
| R6 ↔ R4 | 172.16.14.0/30 | R6: .1 / R4: .2 |

---

## 🔀 VLAN Design

The WAN aggregation switches use separate VLANs for each router connection.

### SW5 — WAN-1

| VLAN | Purpose |
|---|---|
| VLAN 101 | R5 ↔ R1 |
| VLAN 102 | R5 ↔ R2 |
| VLAN 103 | R5 ↔ R3 |
| VLAN 104 | R5 ↔ R4 |

### SW6 — WAN-2

| VLAN | Purpose |
|---|---|
| VLAN 111 | R6 ↔ R1 |
| VLAN 112 | R6 ↔ R2 |
| VLAN 113 | R6 ↔ R3 |
| VLAN 114 | R6 ↔ R4 |

---

## 🛣️ OSPF Routing

OSPF is used as the dynamic routing protocol.

### OSPF Configuration

- OSPF Process ID: `1`
- Area: `0`
- R1 Router ID: `1.1.1.1`
- R2 Router ID: `2.2.2.2`
- R3 Router ID: `3.3.3.3`
- R4 Router ID: `4.4.4.4`
- R5 Router ID: `5.5.5.5`
- R6 Router ID: `6.6.6.6`

OSPF allows the routers to dynamically exchange information about:

- Local LAN networks
- WAN networks
- Remote branch networks

---

## 🔄 How Traffic Flows

Example:

### PC1 → PC3

PC1:

```
10.10.10.10
```

PC3:

```
10.20.20.10
```

PC1 determines that `10.20.20.0/24` is a remote network and forwards the packet to its default gateway:

```
PC1
 ↓
SW1
 ↓
R1
```

R1 uses its OSPF routing table to determine the path toward Branch 1.

Traffic then travels through the WAN:

```
PC1
 ↓
SW1
 ↓
R1
 ↓
R5
 ↓
R2
 ↓
SW2
 ↓
PC3
```

The return traffic follows the reverse routing process.

---

## 🔁 Dual-WAN Architecture

Each site has two WAN connections:

```
             WAN-1
               |
               R5
              /|\
             / | \
            R1 R2 R3 R4
             \ | /
              \|/
               R6
               |
             WAN-2
```

This design introduces WAN redundancy.

The next phase of the project will test:

- WAN-1 failure
- OSPF reconvergence
- WAN-2 path selection
- Network recovery
- Routing table changes

---

## 🧠 Concepts Demonstrated

### Layer 2

- VLANs
- Access ports
- Trunk ports
- 802.1Q
- Router-on-a-stick
- Layer 2 segmentation

### Layer 3

- IPv4 addressing
- Subnetting
- `/30` WAN networks
- Default gateways
- Inter-network routing
- Routing tables

### Dynamic Routing

- OSPF
- OSPF Area 0
- Router IDs
- OSPF neighbor relationships
- Dynamic route advertisement
- Route selection

### Enterprise Networking

- HQ/Branch architecture
- WAN aggregation
- Dual-WAN architecture
- Network segmentation
- Redundancy
- Scalable IP addressing
- Network troubleshooting

---

## 🧪 Verification

Useful commands used during the lab:

```cisco
show ip interface brief
```

```cisco
show ip route
```

```cisco
show ip ospf neighbor
```

```cisco
show ip ospf interface
```

```cisco
show vlan brief
```

```cisco
show interfaces trunk
```

Connectivity testing:

```cisco
ping <destination>
```

```cisco
traceroute <destination>
```

---

## 🛠️ Technologies & Tools

- Cisco Packet Tracer 9.0.1.0
- Cisco IOS
- IPv4
- VLAN
- 802.1Q
- OSPF
- TCP/IP
- Enterprise WAN Architecture

---

## 🚀 Future Enhancements

Planned improvements to this lab:

- [ ] OSPF WAN failover testing
- [ ] OSPF path manipulation
- [ ] DHCP
- [ ] Extended ACLs
- [ ] NAT/PAT
- [ ] SSH hardening
- [ ] STP/RSTP
- [ ] EtherChannel
- [ ] Port Security
- [ ] Syslog
- [ ] NTP
- [ ] SNMP
- [ ] Network monitoring
- [ ] IPsec site-to-site VPN
- [ ] Advanced troubleshooting scenarios

---

## 📁 Project Files

The repository will contain:

```
topology/
configurations/
documentation/
screenshots/
```

The Cisco Packet Tracer `.pkt` file will contain the complete network topology and configurations.

---

## 👨‍💻 Project Purpose

This project was created as a practical enterprise networking lab to develop hands-on skills in network design, routing, WAN architecture, redundancy, troubleshooting, and Cisco IOS configuration.
