# Step 03 - BGP Configuration (Confederation + Route Reflectors + PE-CE)

This step sets up BGP with confederation and route reflectors across the backbone and CE/PE routers, using:

* Confederation structure between sub-ASNs
* Route reflector sessions from R1_RR1 and R2_RR2
* PE-CE BGP or redistribution prep

---

## Confederation Plan

* **Main AS**: 65000 (confederation ID)
* **P Routers**: 65010 (R3, R4)
* **PE Routers**: 65020 (R5, R6)

---

## R1_RR1 (AS 65000)

```bash
router bgp 65000
 bgp router-id 10.255.0.1
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65010 65020
 !
 neighbor 10.255.0.2 remote-as 65010
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 neighbor 10.255.0.2 ebgp-multihop 2
 !
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
 neighbor 10.255.0.3 ebgp-multihop 2
 !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
 neighbor 10.255.0.4 ebgp-multihop 2
 !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 neighbor 10.255.0.5 ebgp-multihop 2
 !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
 neighbor 10.255.0.6 ebgp-multihop 2
 !
 address-family ipv4
  neighbor 10.255.0.2 activate
  neighbor 10.255.0.3 activate
  neighbor 10.255.0.4 activate
  neighbor 10.255.0.5 activate
  neighbor 10.255.0.6 activate
 ```

## R2_RR2 (RR for AS 65000)

```bash
router bgp 65000
 bgp router-id 10.255.0.2
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65010 65020
 !
 neighbor 10.255.0.1 remote-as 65010
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 neighbor 10.255.0.1 ebgp-multihop 2
 !
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
 neighbor 10.255.0.3 ebgp-multihop 2
 !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
 neighbor 10.255.0.4 ebgp-multihop 2
 !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 neighbor 10.255.0.5 ebgp-multihop 2
 !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
 neighbor 10.255.0.6 ebgp-multihop 2
 !
 address-family ipv4
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.3 activate
  neighbor 10.255.0.4 activate
  neighbor 10.255.0.5 activate
  neighbor 10.255.0.6 activate
 ```

## R3_P1 (Sub-AS 65010)

```bash
router bgp 65010
 bgp router-id 10.255.0.3
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65000 65020
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 neighbor 10.255.0.1 ebgp-multihop 2
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 neighbor 10.255.0.2 ebgp-multihop 2
 !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
  !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 neighbor 10.255.0.5 ebgp-multihop 2
  !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
 neighbor 10.255.0.6 ebgp-multihop 2
 !
 address-family ipv4
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.2 activate
  neighbor 10.255.0.4 activate
  neighbor 10.255.0.5 activate
  neighbor 10.255.0.6 activate
```

## R4_P2 (Sub-AS 65010)

```bash
router bgp 65010
 bgp router-id 10.255.0.4
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65000 65020
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 neighbor 10.255.0.1 ebgp-multihop 2
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 neighbor 10.255.0.2 ebgp-multihop 2
 !
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
  !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 neighbor 10.255.0.5 ebgp-multihop 2
  !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
 neighbor 10.255.0.6 ebgp-multihop 2
 !
 address-family ipv4
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.2 activate
  neighbor 10.255.0.3 activate
  neighbor 10.255.0.5 activate
  neighbor 10.255.0.6 activate
```

## R5_PE1 (Sub-AS 65020, PE site 1)

```bash
router bgp 65020
 bgp router-id 10.255.0.5
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65000 65010
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 neighbor 10.255.0.1 ebgp-multihop 2
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 neighbor 10.255.0.2 ebgp-multihop 2
 !
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
 neighbor 10.255.0.3 ebgp-multihop 2
  !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
 neighbor 10.255.0.4 ebgp-multihop 2
  !
 neighbor 10.255.0.6 remote-as 65020
 neighbor 10.255.0.6 update-source Loopback0
 neighbor 10.255.0.6 description "R6_PE2"
 !
 address-family ipv4
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.2 activate
  neighbor 10.255.0.3 activate
  neighbor 10.255.0.4 activate
  neighbor 10.255.0.6 activate
```

## R6_PE2 (Sub-AS 65020, PE site 2)

```bash
router bgp 65020
 bgp router-id 10.255.0.6
 bgp log-neighbor-changes
 bgp confederation identifier 65000
 bgp confederation peers 65000 65010
 neighbor 10.255.0.1 remote-as 65000
 neighbor 10.255.0.1 update-source Loopback0
 neighbor 10.255.0.1 description "R1_RR1"
 neighbor 10.255.0.1 ebgp-multihop 2
 !
 neighbor 10.255.0.2 remote-as 65000
 neighbor 10.255.0.2 update-source Loopback0
 neighbor 10.255.0.2 description "R2_RR2"
 neighbor 10.255.0.2 ebgp-multihop 2
 !
 neighbor 10.255.0.3 remote-as 65010
 neighbor 10.255.0.3 update-source Loopback0
 neighbor 10.255.0.3 description "R3_P1"
 neighbor 10.255.0.3 ebgp-multihop 2
  !
 neighbor 10.255.0.4 remote-as 65010
 neighbor 10.255.0.4 update-source Loopback0
 neighbor 10.255.0.4 description "R4_P2"
 neighbor 10.255.0.4 ebgp-multihop 2
  !
 neighbor 10.255.0.5 remote-as 65020
 neighbor 10.255.0.5 update-source Loopback0
 neighbor 10.255.0.5 description "R5_PE1"
 !
 address-family ipv4
  neighbor 10.255.0.1 activate
  neighbor 10.255.0.2 activate
  neighbor 10.255.0.3 activate
  neighbor 10.255.0.4 activate
  neighbor 10.255.0.5 activate
```