# Step 07 - Redistribution: BGP ↔ OSPF (VRF-Lite)

This step configures route redistribution between BGP and OSPF inside the `CUSTOMER1` VRF on PE routers (R5_PE1 and R6_PE2).

`set tag` is buggy, not sure why - oppdater i morgen

---

## Tag Strategy (Optional)

| Direction  | Tag | Purpose                |
| ---------- | --- | ---------------------- |
| BGP → OSPF | 100 | Mark external routes   |
| OSPF → BGP | —   | No tagging, match only |

---

## Route-map for OSPF → BGP Filtering

```bash
route-map OSPF_TO_BGP deny 10
 match tag 100
!
route-map OSPF_TO_BGP permit 20
```

---

## R5_PE1 Configuration

```bash
router ospf 100 vrf CUSTOMER1
 redistribute bgp 65020 subnets
!
router bgp 65020
 address-family ipv4 vrf CUSTOMER1
  redistribute ospf 100 route-map OSPF_TO_BGP
```

---

## R6_PE2 Configuration

```bash
router ospf 100 vrf CUSTOMER1
 redistribute bgp 65020 subnets
!
router bgp 65020
 address-family ipv4 vrf CUSTOMER1
  redistribute ospf 100 route-map OSPF_TO_BGP
```

---

## Verification

```bash
show ip route vrf CUSTOMER1
show ip protocols vrf CUSTOMER1
show bgp ipv4 vrf CUSTOMER1
```