# Step 01 - Interface and Loopback Configuration

Configure all interfaces and Loopback0 IPs on each router based on the IP Plan.
This step ensures reachability and prepares for routing protocols.

---

## R1_RR1 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.0.1 255.255.255.255
!
interface GigabitEthernet1
 description "To R3_PE1"
 ip address 10.0.0.1 255.255.255.252
 no shutdown
```

## R2_RR2 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.0.2 255.255.255.255
!
interface GigabitEthernet1
 description "To R4_PE2"
 ip address 10.0.0.5 255.255.255.252
 no shutdown
```

## R3_P1 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.0.3 255.255.255.255
!
interface GigabitEthernet1
 description "To R1_RR1"
 ip address 10.0.0.2 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To R4_PE2"
 ip address 10.0.0.9 255.255.255.252
 no shutdown
interface GigabitEthernet3
 description "To R5_PE1"
 ip address 10.0.0.13 255.255.255.252
 no shutdown
```

## R4_P2 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.0.4 255.255.255.255
!
interface GigabitEthernet1
 description "To R2_RR2"
 ip address 10.0.0.6 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To R3_P1"
 ip address 10.0.0.10 255.255.255.252
 no shutdown
interface GigabitEthernet3
 description "To R6_PE2"
 ip address 10.0.0.17 255.255.255.252
 no shutdown
```

## R5_PE1 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.0.5 255.255.255.255
!
interface GigabitEthernet1
 description "To R3_P1"
 ip address 10.0.0.14 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To R7_CE1"
 ip address 10.0.1.1 255.255.255.252
 no shutdown
```

## R6_PE2 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.0.6 255.255.255.255
!
interface GigabitEthernet1
 description "To R4_PE2"
 ip address 10.0.0.18 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To R8_CE2"
 ip address 10.0.1.5 255.255.255.252
 no shutdown
```

## R7_CE1 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.1.1 255.255.255.255
!
interface GigabitEthernet1
 description "To R5_PE1"
 ip address 10.0.1.2 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To R9_C1"
 ip address 10.0.1.9 255.255.255.252
 no shutdown
```

## R8_CE2 (IOSvCSR1000v)

```bash
interface Loopback0
 ip address 10.255.1.2 255.255.255.255
!
interface GigabitEthernet1
 description "To R6_PE2"
 ip address 10.0.1.6 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To R10_C2"
 ip address 10.0.1.13 255.255.255.252
 no shutdown
```

## R9_C1 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.2.1 255.255.255.255
!
interface GigabitEthernet1
 description "To R7_CE1"
 ip address 10.0.1.10 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To ASW1"
 ip address 10.0.1.17 255.255.255.252
 no shutdown
interface GigabitEthernet3
 description "ROAS"
 no shutdown
```

## R10_C2 (CSR1000v)

```bash
interface Loopback0
 ip address 10.255.2.2 255.255.255.255
!
interface GigabitEthernet1
 description "To R8_CE2"
 ip address 10.0.1.14 255.255.255.252
 no shutdown
interface GigabitEthernet2
 description "To ASW1"
 ip address 10.0.1.18 255.255.255.252
 no shutdown
interface GigabitEthernet3
 description "ROAS"
 no shutdown