# Step 09 - Enhancements: CoPP, SNMPv3, Syslog, DHCP (VRF-Lite)

This step includes enhancements to harden and monitor the network in the VRF-Lite environment.

---
 
## Control Plane Policing (CoPP) on core and PE routers

# Because CoPP is not fully supported on CSR1000v in EVE-NG; I will simulate CoPP by using ACLs.

CoPP for protecting the router CPU from excessive traffic.

```bash
access-list 100 permit ospf any any
access-list 100 permit tcp any any eq 179

class-map match-any ROUTING-TRAFFIC
 match access-group 100
!
policy-map CoPP
 class ROUTING-TRAFFIC
  police 10000000 64000 conform-action transmit exceed-action drop  # 10Mbps with 64kB burst
!
interface GigabitEthernet1 #Include all in-use port(s).
 service-policy input CoPP
```

---

## SNMPv3 Configuration (on R5_PE1 and R6_PE2)

```bash
snmp-server group GOLRANG v3 priv
snmp-server user admin GOLRANG v3 auth sha GOLPER priv aes 256 GOLPER
snmp-server location "PE"
snmp-server contact netadmin@golrang.local
```

SNMPv3 is used with the monitoring tool "LibreNMS".

---

```bash
logging 10.10.10.100     # Ubuntu host as syslog server
logging trap warnings
service timestamps log datetime msec
```

---

## NTP Configuration (on R5_PE1 and R6_PE2)

```bash
ntp server 10.10.10.100 prefer
```

---

## DHCP Relay (on R9_C1)

ROAS is used, so `ip helper-address` must be applied directly on the subinterface:

```bash
interface GigabitEthernet3.10
 ip helper-address 10.10.10.100
```
```bash
interface GigabitEthernet3.99
 ip helper-address 10.10.10.100
```


---

## DHCP Relay (R10_C2)
```bash
interface GigabitEthernet3.10
 ip helper-address 10.10.10.100
```

```bash
interface GigabitEthernet3.20
 ip helper-address 10.10.10.100
```

```bash
interface GigabitEthernet3.99
 ip helper-address 10.10.10.100
```
