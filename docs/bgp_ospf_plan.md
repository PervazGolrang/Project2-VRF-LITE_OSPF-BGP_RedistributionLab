# BGP & OSPF Plan

## BGP Autonomous System Numbers (ASNs)

| Device  | ASN   | Notes                              |
| ------- | ----- | ---------------------------------- |
| R1_RR1  | 65000 | Confederation Identifier (Main AS) |
| R2_RR2  | 65000 | Confederation Identifier (Main AS) |
| R3_P1   | 65010 | Confederation Sub-AS (iBGP member) |
| R4_P2   | 65010 | Confederation Sub-AS               |
| R5_PE1  | 65020 | Confederation Sub-AS               |
| R6_PE2  | 65020 | Confederation Sub-AS               |
| R7_CE1  | 65100 | Customer ASN (BGP PE-CE or OSPF)   |
| R8_CE2  | 65100 | Customer ASN                       |
| R9_C1   | —     | Not part of BGP                    |
| R10_C2  | —     | Not part of BGP                    |

### Notes:

* Confederation used to simulate scalable SP BGP
* BGP used between PE and CE (or OSPF redistributed)

---

## OSPF Area Assignments

| Link / Devices    | Area     | Notes                                  |
| ----------------- | -------- | -------------------------------------- |
| Core (R1–R6)      | 0        | Backbone area                          |
| R5 ↔ R7           | 100      | Transit area for virtual-link          |
| R7 ↔ R9           | 100      | Stub area                              |
| R6 ↔ R8 ↔ R10     | 200      | NSSA area                              |

### Notes:

* Virtual link required to bring CE1 (R7) into backbone via R5      # Possibly to be removed
* Stub area used for simplicity at site 1                           # Possibly to be removed
* NSSA area used at site 2 to test type-7 to type-5 redistribution  # Possibly to be removed