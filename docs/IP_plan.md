# IP Address and Loopback Plan

## Loopback Addresses (for OSPF Router-ID, BGP Peering)

| Device  | Loopback0 IP  | Purpose             |
| ------- | ------------- | ------------------- |
| R1_RR1  | 10.255.0.1/32 | BGP RR Router-ID    |
| R2_RR2  | 10.255.0.2/32 | BGP RR Router-ID    |
| R3_P1   | 10.255.0.3/32 | MPLS Core Router-ID |
| R4_P2   | 10.255.0.4/32 | MPLS Core Router-ID |
| R5_PE1  | 10.255.0.5/32 | PE Router 1         |
| R6_PE2  | 10.255.0.6/32 | PE Router 2         |
| R7_CE1  | 10.255.1.1/32 | Customer Site 1     |
| R8_CE2  | 10.255.1.2/32 | Customer Site 2     |
| R9      | 10.255.2.1/32 | Branch 1            |
| R10     | 10.255.2.2/32 | Branch 2            |

---

## Point-to-Point Interfaces (for OSPF + MPLS + PE-CE)

| Link           | Interface (A) | Interface (B) | IP A      | IP B      | Subnet       | Area |
| -------------- | ------------- | ------------- | --------- | --------- | ------------ | ---- |
| R1 ↔ R3        | R1 Gi1        | R3 Gi1        | 10.0.0.1  | 10.0.0.2  | 10.0.0.0/30  | 0    |
| R2 ↔ R4        | R2 Gi1        | R4 Gi1        | 10.0.0.5  | 10.0.0.6  | 10.0.0.4/30  | 0    |
| R3 ↔ R4        | R3 Gi2        | R4 Gi2        | 10.0.0.9  | 10.0.0.10 | 10.0.0.8/30  | 0    |
| R3 ↔ R5        | R3 Gi3        | R5 Gi1        | 10.0.0.13 | 10.0.0.14 | 10.0.0.12/30 | 0    |
| R4 ↔ R6        | R4 Gi3        | R6 Gi1        | 10.0.0.17 | 10.0.0.18 | 10.0.0.16/30 | 0    |
| R5 ↔ R7        | R5 Gi2        | R7 Gi1        | 10.0.1.1  | 10.0.1.2  | 10.0.1.0/30  | 51   |
| R6 ↔ R8        | R6 Gi2        | R8 Gi1        | 10.0.1.5  | 10.0.1.6  | 10.0.1.4/30  | 200  |
| R7 ↔ R9        | R7 Gi2        | R9 Gi1        | 10.0.1.9  | 10.0.1.10 | 10.0.1.8/30  | 100  |
| R8 ↔ R10       | R8 Gi2        | R10 Gi1       | 10.0.1.13 | 10.0.1.14 | 10.0.1.12/30 | 200  |
| R9 ↔ R10 (VPN) | R9 Gi2        | R10 Gi2       | 10.0.1.17 | 10.0.1.18 | 10.0.1.16/30 | N/A  |

---

## LAN Segments (VLANs / Hosts)

| Device    | Interface    | IP           | Subnet        | Notes           |
| --------- | ------------ | ------------ | ------------- | --------------- |
| PC1       | eth0         | 10.10.10.10  | 10.10.10.0/24 | VLAN 10 (SW1)   |
| Ubuntu    | eth0         | 10.10.10.100 | 10.10.10.0/24 | VLAN 10         |
| R9 (SVI)  | Gi2 (ROAS)   | 10.10.10.1   | 10.10.10.0/24 | Default Gateway |
| PC2       | eth0         | 10.10.20.10  | 10.10.20.0/24 | VLAN 20 (SW2)   |
| R10 (SVI) | Gi2 (ROAS)   | 10.10.20.1   | 10.10.20.0/24 | Default Gateway |
