# Project 2 – VRF-Lite, OSPF, BGP, Redistribution & VPN

## Overview

This project simulates a service provider-style environment integrating OSPF, BGP (with confederations), VRF-lite, IPsec VPN tunnels, redistribution between routing protocols, and core enterprise LAN features. The lab is built entirely in EVE-NG using Cisco IOSv-L2, CSR1000v, and a Linux node. The lab includes:

* BGP with confederation
* Multi-area OSPF with stub/NSSA areas
* Basic VRF-Lite without MPLS
* Redistribution between OSPF and BGP with loop prevention
* Branch IPsec VPN tunnel with IKEv2
* LAN switching with STP, LACP
* Syslog/SNMP testing with Ubuntu

---

## Platform

* **Emulator:** EVE-NG
* **Router Images:**

  * CSR1000v
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

### VRF-Lite

* VRF configuration on R5 and R6

### BGP

* iBGP with RR: R1 and R2
* BGP Confederation: R1–R6 split into sub-ASes
* CE–PE OSPF

### Security

* IPsec IKEv2 VPN tunnel between R9 and R10
* Redistribution at PE with route maps and tagging
* VRRP between R9 and R10 (gateway failover)
* CoPP and PBR

---

## Network Topology

![`Network Topology`](topology/project2_topology-eve_ng.png)

### Drawio Topology
[`project2_topology.drawio`](topology/project2_topology.drawio)  

---

## Directory Layout

```
PROJECT2-VRF-LITE_OSPF_BGP_REDISTRIBUTION_LAB/
├── configs/            # Final .txt configs for each device
├── docs/               # Diagrams, reference PDFs, removed steps
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

* Achieve full end-to-end reachability across BGP/OSPF
* Validate redistribution and loop-prevention route-map
* Demonstrate routing behavior and CoPP **interface** protection
* Test PC reachability and log/syslog features via Ubuntu
* Capture packets to validate tunnels and routing

---

## Author notes

Lab is created for skill demonstration in CCNP-level enterprise + provider design with realistic scenarios and full documentation.
