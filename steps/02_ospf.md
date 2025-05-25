# Step 02 - OSPF Configuration

Configure OSPF across all routers with correct area assignments, passive-interface defaults, loopback advertisement, and virtual-link setup.

---

## R1_RR1

```bash
router ospf 1
 router-id 10.255.0.1
 network 10.255.0.1 0.0.0.0 area 0
 network 10.0.0.0 0.0.0.3 area 0
 passive-interface default
 no passive-interface GigabitEthernet1
```

## R2_RR2

```bash
router ospf 1
 router-id 10.255.0.2
 network 10.255.0.2 0.0.0.0 area 0
 network 10.0.0.4 0.0.0.3 area 0
 passive-interface default
 no passive-interface GigabitEthernet1
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
```

## R5_PE1

```bash
router ospf 1
 router-id 10.255.0.5
 network 10.255.0.5 0.0.0.0 area 0
 network 10.0.0.12 0.0.0.3 area 0
 network 10.0.1.0 0.0.0.3 area 51
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
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
```

## R7_CE1

```bash
router ospf 1
 router-id 10.255.1.1
 network 10.255.1.1 0.0.0.0 area 100
 network 10.0.1.2 0.0.0.0 area 51
 network 10.0.1.8 0.0.0.3 area 100
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
```

## R8_CE2

```bash
router ospf 1
 router-id 10.255.1.2
 network 10.255.1.2 0.0.0.0 area 200
 network 10.0.1.6 0.0.0.0 area 200
 network 10.0.1.12 0.0.0.3 area 200
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet2
```

## R9_C1 (DR/BDR Area 100)

```bash
router ospf 1
 router-id 10.255.2.1
 network 10.255.2.1 0.0.0.0 area 100
 network 10.0.1.10 0.0.0.0 area 100
 network 10.10.10.1 0.0.0.0 area 100
 passive-interface default
 no passive-interface GigabitEthernet1
 no passive-interface GigabitEthernet3
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
 no passive-interface GigabitEthernet3
```

---

## Virtual Link

### On R5_PE1:

```bash
area 51 virtual-link 10.255.1.1
```

### On R7_CE1:

```bash
area 51 virtual-link 10.255.0.5
```
