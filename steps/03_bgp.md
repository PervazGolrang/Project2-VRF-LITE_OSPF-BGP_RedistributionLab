# Step 03 - BGP Configuration (Confederation + Route Reflectors + PE-CE)

This step establishes full BGP across the backbone and CE/PE, using:

* Confederation structure between sub-ASNs
* Route reflector sessions from R1_RR1 and R2_RR2
* PE-CE BGP or redistribution prep

# MEMO: Add RR til BGP - gjøres i morgen

---

## Confederation Plan

* **Main AS**: 65000 (confederation ID)
* **Sub-AS**: 65010 (R3, R4), 65020 (R5, R6)
* **Customer AS**: 65100 (R7, R8)

---

## R1_RR1 (RR for AS 65000)

```bash
router bgp 65000
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65010 65020
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
 !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
 !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
```

## R2_RR2 (RR for AS 65000)

```bash
router bgp 65000
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65010 65020
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
 !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
 !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
```

## R3_P1 (Sub-AS 65010)

```bash
router bgp 65010
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
```

## R4_P2 (Sub-AS 65010)

```bash
router bgp 65010
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 !
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
```

## R5_PE1 (Sub-AS 65020, PE site 1)

```bash
router bgp 65020
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
```

## R6_PE2 (Sub-AS 65020, PE site 2)

```bash
router bgp 65020
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
```