# Step 02 - OSPF Configuration

Configure OSPF across all routers with correct area assignments, passive-interface defaults, and loopback advertisement.

Configuration of OSPF with three areas:

- **Area 0**: Provider and CE routers (core and edge backbone)
- **Area 100**: Customer Site 1 (CE1 and C1)
- **Area 200**: Customer Site 2 (CE2 and C2)

- **Area 100** = NSSA (R5–R7–R9)
- **Area 200** = Stub (R6–R8–R10)

---

## R1_RR1

```bash
router ospf 1
 router-id 10.255.0.1
 network 10.255.0.1 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.3 area 0
 passive-interface default
 no passive-interface GigabitEthernet1
!
interface GigabitEthernet1
 ip ospf network point-to-point
```

## R2_RR2

```bash
router ospf 1
 router-id 10.255.0.2
 network 10.255.0.2 0.0.0.0 area 0
 network 10.0.0.4 0.0.0.3 area 0
 passive-interface default
 no passive-interface GigabitEthernet1
!
interface GigabitEthernet1
 ip ospf network point-to-point
```

## R3_P1

```bash
router ospf 1
 router-id 10.255.0.3
 network 10.255.0.3 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.255 area 0
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
 no passive-interface GigabitEthernet3
!
interface range GigabitEthernet1-3
 ip ospf network point-to-point
```

## R4_P2

```bash
router ospf 1
 router-id 10.255.0.4
 network 10.255.0.4 0.0.0.0 area 0
 network 10.0.0.4 0.0.0.255 area 0
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
 no passive-interface GigabitEthernet3
!
interface range GigabitEthernet1-3
 ip ospf network point-to-point
```

## R5_PE1

```bash
router ospf 1
 router-id 10.255.0.5
 network 10.255.0.5 0.0.0.0 area 0
 network 10.0.0.12 0.0.0.3 area 0
 network 10.0.1.0 0.0.0.3 area 100
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
!
interface range GigabitEthernet1-2
 ip ospf network point-to-point
```

## R6_PE2

```bash
router ospf 1
 router-id 10.255.0.6
 network 10.255.0.6 0.0.0.0 area 0
 network 10.0.0.16 0.0.0.3 area 0
 network 10.0.1.4 0.0.0.3 area 200
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
!
interface range GigabitEthernet1-2
 ip ospf network point-to-point
```

## R7_CE1

```bash
router ospf 1
 router-id 10.255.1.1
 network 10.255.1.1 0.0.0.0 area 100
 network 10.0.1.0 0.0.0.3 area 100
 network 10.0.1.8 0.0.0.3 area 100
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
!
interface range GigabitEthernet1
 ip ospf network point-to-point
```

## R8_CE2

```bash
router ospf 1
 router-id 10.255.1.2
 network 10.255.1.2 0.0.0.0 area 200
 network 10.0.1.6 0.0.0.0 area 200
 network 10.0.1.12 0.0.0.3 area 200
 passive-interface default
 area 100 nssa
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
!
interface range GigabitEthernet1
 ip ospf network point-to-point
```

## R9_C1 (DR/BDR Area 100)

```bash
router ospf 1
 router-id 10.255.2.1
 network 10.255.2.1 0.0.0.0 area 100
 network 10.0.1.10 0.0.0.0 area 100
 network 10.10.10.1 0.0.0.0 area 100
 passive-interface default
 area 200 stub
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
```

## R10_C2 (DR/BDR Area 200)

```bash
router ospf 1
 router-id 10.255.2.2
 network 10.255.2.2 0.0.0.0 area 200
 network 10.0.1.14 0.0.0.0 area 200
 network 10.10.20.1 0.0.0.0 area 200
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
```