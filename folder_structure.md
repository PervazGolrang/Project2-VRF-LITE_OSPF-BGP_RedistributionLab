# Folder Structure – Project 2

```
PROJECT2-MPLS_OSPF_BGP_REDISTRIBUTIONLAB/
├── captures/                # Wireshark .pcap files (exported from EVE-NG)
├── configs/                 # Final full device CLI configs (.txt)
│   ├── R1_RR1.txt
│   ├── R2_RR2.txt
│   └── ...
├── configs_yaml/            # Same configs in structured YAML format (optional)
├── docs/                    # Exports of diagrams, PDFs, reference screenshots
|   └── ip_plan.md 
├── notes/                   # Notes and scratchpad for ideas during lab work
│   └── notes.md
├── steps/                   # Ordered configuration steps per phase
│   ├── 01_interfaces.md
│   ├── 02_ospf.md
│   ├── 03_bgp.md
│   ├── 04_mpls.md
│   ├── 05_vrf_l3vpn.md
│   ├── 06_ipsec.md
│   ├── 07_redistribution.md
│   ├── 08_switching_vlan.md
│   └── 09_optional_tuning.md
├── topology/                # .drawio and/or .top files for the full lab layout
│   ├── project2_topology.drawio
│   └── project2_topology-eve_ng.png
├── .gitattributes
├── LICENSE
├── README.md
├── bgp_ospf_plan.md
└── folder_structure.md
```

## Notes

* All config files in `configs/` will reflect final working state per device
* `steps/` folder is meant to mirror config phase-by-phase
* `.drawio` files should match what’s built in EVE-NG exactly
* `captures/` folder should be used to validate MPLS, VPN, or routing events