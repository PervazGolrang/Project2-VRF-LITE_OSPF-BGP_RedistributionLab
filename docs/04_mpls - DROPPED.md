# Step 04 – MPLS and LDP Configuration

This step enables MPLS across the core network. LDP is configured to distribute labels over OSPF neighbors between P and PE routers. 

# NOTE:
* This step has been removed due to lab limitations.

---

## R3_P1 (Core Router)

```bash
mpls ip
!
interface GigabitEthernet1
 mpls ip
!
interface GigabitEthernet2
 mpls ip
!
interface GigabitEthernet3
 mpls ip
```

## R4_P2 (Core Router)

```bash
mpls ip
!
interface GigabitEthernet1
 mpls ip
!
interface GigabitEthernet2
 mpls ip
!
interface GigabitEthernet3
 mpls ip
```

## R5_PE1 (Provider Edge 1)

```bash
mpls ip
!
interface GigabitEthernet1
 mpls ip
!
interface GigabitEthernet2
 mpls ip
```

## R6_PE2 (Provider Edge 2)

```bash
mpls ip
!
interface GigabitEthernet1
 mpls ip
!
interface GigabitEthernet2
 mpls ip 
```

---

## LDP Configuration (R3-R6)

```bash
mpls label protocol ldp
mpls ldp router-id Loopback0 force
```

---

## MPLS Verification Commands

```bash
show mpls ldp neighbor
show mpls forwarding-table
show mpls interfaces
```