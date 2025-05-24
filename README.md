# Project 2 – MPLS, OSPF, BGP, Redistribution & VPN Lab

## Overview

This project simulates a complex service provider-style environment integrating MPLS, OSPF, BGP (with route reflectors and confederations), L3VPN with VRF, IPsec VPN tunnels, redistribution between routing protocols, and core enterprise LAN features. The lab is built entirely in EVE-NG using Cisco IOSv, IOSv-L2, CSR1000v, and a Linux node. The lab includes:

* BGP with confederation and route reflection
* Multi-area OSPF with virtual link and stub/NSSA areas
* MPLS core with L3VPN (VRF, RD/RT)
* Sham-link between PE routers
* Redistribution between OSPF and BGP with loop prevention
* Branch IPsec VPN tunnel with IKEv2
* LAN switching with STP, LACP, and DR/BDR election
* Syslog/SNMP testing with Ubuntu

---

## Platform

* **Emulator:** EVE-NG
* **Router Images:**

  * CSR1000v
  * IOSv
* **Switch Images:**

  * IOSv-L2
* **Client/Host:**

  * VPCS (for PC1, PC2)
  * Ubuntu 22.04 LTS

---

## Features

### Core Routing

* OSPF Area 0 (Backbone): R1–R6
* Stub/NSSA Areas: Area 100 (R7–R9), Area 200 (R8–R10)
* Area 51 (transit for virtual-link between R5 and R7)
* DR/BDR election: R9 ↔ SW1, R10 ↔ SW2

### MPLS & L3VPN

* MPLS LDP on R3 and R4
* VRF configuration on R5 and R6
* Sham-link configured across PE1 and PE2
* Route Distinguisher and Route Target for VRF

### BGP

* iBGP with RR: R1 and R2
* BGP Confederation: R1–R6 split into sub-ASes
* CE–PE BGP or OSPF (varies by site)

### Security

* IPsec IKEv2 VPN tunnel between R9 and R10
* Redistribution at PE with route maps and tagging
* VRRP between R9 and R10 (gateway failover)
* CoPP and PBR

---

## Topology
[`Topology_drawio.drawio`](topology/project2_topology.drawio)
[`Topology_eve-ng.png`](topology/project2_topology-eve_ng.png)

---

## Directory Layout

```
PROJECT2-MPLS_OSPF_BGP_REDIS_LAB/
├── captures/           # .pcap files from Wireshark/EVE-NG
├── configs/            # Final .txt configs for each device
├── configs_yaml/       # Same configs in .yaml (structured)
├── docs/               # Diagrams, reference PDFs
├── notes/
│   └── notes.md        # Free-form implementation notes
├── steps/              # Step-by-step setup instructions
├── topology/           # .drawio and/or .top files
├── LICENSE
├── .gitattributes
├── folder_structure.md # Folder structure
└── README.md           # This file
```

---

## Lab Goals

* Achieve full end-to-end reachability across MPLS/BGP/OSPF
* Validate redistribution and route tagging
* Demonstrate VPN failover and routing behavior
* Test PC reachability and log/syslog features via Ubuntu
* Capture packets to validate tunnels and routing

---

## Author notes

Lab is created for skill demonstration in CCNP-level enterprise + provider design with realistic scenarios and full documentation.