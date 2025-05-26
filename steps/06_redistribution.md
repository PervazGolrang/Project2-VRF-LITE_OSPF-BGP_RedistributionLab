# Step 06 - Redistribution: BGP ↔ OSPF (VRF-Lite)

This step configures controlled redistribution between BGP and OSPF inside the `CUSTOMER1` VRF on PE routers (R5_PE1 and R6_PE2). Redistribution is required to allow CE routers (OSPF) to reach the core (BGP) and vice versa.

---

## Tag-Based Filtering Overview

| Direction  | Route Map Name | Purpose                        | Tag |
| ---------- | -------------- | ------------------------------ | --- |
| BGP → OSPF | `BGP_TO_OSPF`  | Add tag 100 to injected routes | 100 |
| OSPF → BGP | `OSPF_TO_BGP`  | Drop any route with tag 100    | 100 |

This prevents re-injecting the same routes back into the source protocol (a.k.a loop prevention).

---

## Configuration on R5_PE1 and R6_PE2

### 1. Create the route-maps

```bash
route-map BGP_TO_OSPF permit 10
 set tag 100
!
route-map OSPF_TO_BGP deny 10
 match tag 100
!
route-map OSPF_TO_BGP permit 20
```

---

### 2. Apply the route-maps in redistribution

#### On both R5_PE1 and R6_PE2:

```bash
router ospf 100 vrf CUSTOMER1
 redistribute bgp 65020 subnets route-map BGP_TO_OSPF
!
router bgp 65020
 address-family ipv4 vrf CUSTOMER1
  redistribute ospf 100 route-map OSPF_TO_BGP
```

---

## Verification Commands

#### On both R5_PE1 and R6_PE2:

```bash
show ip route vrf CUSTOMER1
show ip protocols vrf CUSTOMER1
show route-map
```

You should see:

* BGP routes with tag 100 injected into OSPF
* OSPF routes accepted into BGP (except tag 100)
* OSPF routes arriving at CE (from PE redistribution)

From CE1 and CE2:

```bash
show ip route
show ip ospf neighbor
```