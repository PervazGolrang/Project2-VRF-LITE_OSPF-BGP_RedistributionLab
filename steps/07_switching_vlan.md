# Step 08 - VLAN, ROAS, VRRP, and L2 Switch Configuration

This step sets up:

* VLANs (10 for PC1, 20 for PC2, 99 for PC1/PC2)
* ROAS on R9/R10 for inter-VLAN routing
* VRRP for gateway redundancy
* STP, portfast, and basic security on switches

---

## VLAN and ROAS (Router-on-a-Stick)

### R9_C1 (Site 1)

The management VLAN is optional, however, I see it as best practice.

```bash
interface GigabitEthernet3.10
 encapsulation dot1Q 10
 ip address 10.10.10.1 255.255.255.0
!
interface GigabitEthernet3.99
 encapsulation dot1Q 99
 ip address 10.10.99.1 255.255.255.0    # MGMT VLAN
```

### R10_C2 (Site 2)

```bash
interface GigabitEthernet3.20
 encapsulation dot1Q 20
 ip address 10.10.20.1 255.255.255.0
 !
 interface GigabitEthernet3.99
 encapsulation dot1Q 99
 ip address 10.10.99.1 255.255.255.0    # MGMT VLAN
```

---

## VRRP for Gateway Redundancy

### R9_C1 (VLAN 10)

```bash
interface GigabitEthernet3.10
 vrrp 10 ip 10.10.10.254
 vrrp 10 priority 110
 vrrp 10 preempt
```

### R10_C2 (VLAN 10)

```bash
interface GigabitEthernet3.10
 encapsulation dot1Q 10
 ip address 10.10.10.2 255.255.255.0
 vrrp 10 ip 10.10.10.254
 vrrp 10 priority 100
 vrrp 10 preempt
```

---

## Switch Configuration: SW1 & SW2

### Global Settings (both switches)

```bash
spanning-tree mode rapid-pvst
!
vtp mode transparent
```

### Access VLAN Ports

```bash
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface GigabitEthernet0/2          # Only for ASW1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
 spanning-tree bpduguard enable
```

### Trunk to Router

```bash
interface GigabitEthernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
```

---

## Port Security

```bash
interface range GigabitEthernet0/1-2  #Include g0/2 only on ASW
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation restrict
 switchport port-security mac-address sticky
```