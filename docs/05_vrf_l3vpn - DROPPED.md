# Step 05 - VRF and L3VPN Configuration

This (unfinished) step defines VRFs on PE routers, assigns interfaces to VRFs, configures route distinguishers and route targets, and connects each site into the L3VPN. A sham-link is also configured for intra-VRF OSPF continuity between sites.

# NOTE:
* This step has been removed due to lab limitations.

---

## VRF Definition on PE Routers (R5_PE1 and R6_PE2)

### R5_PE1

```bash
ip vrf CUSTOMER1
 rd 65100:1
 route-target export 65100:1
 route-target import 65100:1
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
 route-target export 65100:1
 route-target import 65100:1
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

## Routing Inside VRF (OSPF with Sham-Link)

### R5_PE1

```bash
router ospf 100 vrf CUSTOMER1
 router-id 10.255.0.5
 network 10.0.1.0 0.0.0.3 area 100
 network 192.168.10.1 0.0.0.0 area 100
!
interface Tunnel100
 ip vrf forwarding CUSTOMER1
 ip address 192.168.100.1 255.255.255.252
 tunnel source Loopback10
 tunnel destination 192.168.10.2
 tunnel mode ipsec ipv4
!
router ospf 100 vrf CUSTOMER1
 area 100 sham-link 10.255.0.5 10.255.0.6
```

### R6_PE2

```bash
router ospf 100 vrf CUSTOMER1
 router-id 10.255.0.6
 network 10.0.1.4 0.0.0.3 area 100
 network 192.168.10.2 0.0.0.0 area 100
!
interface Tunnel100
 ip vrf forwarding CUSTOMER1
 ip address 192.168.100.2 255.255.255.252
 tunnel source Loopback10
 tunnel destination 192.168.10.1
 tunnel mode ipsec ipv4
!
router ospf 100 vrf CUSTOMER1
 area 100 sham-link 10.255.0.6 10.255.0.5
```

---

## Redistribution (PE-CE)

```bash
router ospf 100 vrf CUSTOMER1
 redistribute bgp 65020 subnets
```