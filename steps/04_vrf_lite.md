# Step 05 - VRF-Lite Configuration

This step configures VRF separation on the PE routers (R5_PE1 and R6_PE2) without using MPLS or BGP VPNv4. Each site is isolated into its own VRF, and routing is exchanged with the CE routers using OSPF and BGP (per VRF).

---

## VRF Definition on PE Routers

### R5_PE1

```bash
ip vrf CUSTOMER1
 rd 65100:1
!
interface GigabitEthernet2
 ip vrf forwarding CUSTOMER1
 ip address 10.0.1.1 255.255.255.252
 no shutdown
!
interface Loopback10
 ip vrf forwarding CUSTOMER1
 ip address 192.168.10.1 255.255.255.255
```

### R6_PE2

```bash
ip vrf CUSTOMER1
 rd 65100:1
!
interface GigabitEthernet2
 ip vrf forwarding CUSTOMER1
 ip address 10.0.1.5 255.255.255.252
 no shutdown
!
interface Loopback10
 ip vrf forwarding CUSTOMER1
 ip address 192.168.10.2 255.255.255.255
```

---

## OSPF Routing Inside VRF

### R5_PE1

```bash
router ospf 100 vrf CUSTOMER1
 router-id 10.255.0.5
 network 10.0.1.0 0.0.0.3 area 100
 network 192.168.10.1 0.0.0.0 area 100
 redistribute bgp 65020 subnets
```

### R6_PE2

```bash
router ospf 100 vrf CUSTOMER1
 router-id 10.255.0.6
 network 10.0.1.4 0.0.0.3 area 100
 network 192.168.10.2 0.0.0.0 area 100
 redistribute bgp 65020 subnets
```

---

## BGP per-VRF Routing

### R5_PE1

```bash
router bgp 65020
 address-family ipv4 vrf CUSTOMER1
  redistribute ospf 100
```

### R6_PE2

```bash
router bgp 65020
 address-family ipv4 vrf CUSTOMER1
  redistribute ospf 100
```

---

## Summary

This setup isolates customer traffic using VRFs (without MPLS). Each PE router runs both OSPF and BGP within the VRF to support route exchange and redistribution.

VPNv4, route-targets, and LDP are not required.
