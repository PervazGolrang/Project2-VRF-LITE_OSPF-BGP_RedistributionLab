# Folder Structure – Project 2

```
PROJECT2-VRF-LITE_OSPF_BGP_REDISTRIBUTION_LAB/
├── configs/                 # Final full device CLI configs (.txt)
│   ├── R1_RR1.txt
│   ├── R2_RR2.txt
│   └── ...
├── docs/                    # Exports of diagrams, PDFs, removed steps
|   ├── ip_plan.md
│   ├── bgp_ospf_plan.md
│   ├── 04_mpls - DROPPED.md
│   └── 05_vrf_l3vpn - DROPPED.md
├── notes/                   # Notes and scratchpad for ideas during lab work
│   └── notes.md
├── steps/                   # Ordered configuration steps per phase
│   ├── 01_interfaces.md
│   ├── 02_ospf.md
│   ├── 03_bgp.md
│   ├── 04_vrf_lite.md
│   ├── 05_ipsec.md
│   ├── 06_redistribution.md
│   ├── 07_switching_vlan.md
│   ├── 08_senhancements.md
│   └── 09_ubuntu.md
├── topology/                # .drawio and/or .top files for the full lab layout
│   ├── project2_topology.drawio
│   └── project2_topology-eve_ng.png
├── .gitattributes
├── LICENSE
├── README.md
└── folder_structure.md
```

## Notes

* All config files in `configs/` will reflect final working state per device
* `steps/` folder is meant to mirror config phase-by-phase
* `.drawio` files should match what’s built in EVE-NG exactly
* `captures/` folder should be used to validate MPLS, VPN, or routing events