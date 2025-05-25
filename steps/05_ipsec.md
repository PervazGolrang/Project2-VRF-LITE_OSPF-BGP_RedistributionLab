# Step 06 - IPsec VPN with IKEv2 (R9 ↔ R10)

This step configures an IKEv2-based IPsec tunnel between R9 and R10. The tunnel is used for secure communication between branch sites.

---

## Phase 1: IKEv2 Configuration (R9_C1 & R10_C2)

### Common IKEv2 Proposal (Both Routers)

```bash
crypto ikev2 proposal IKEV2-PROPOSAL
 encryption aes-cbc-256
 integrity sha256
 group 19
```

### IKEv2 Policy (Both Routers)

```bash
crypto ikev2 policy IKEV2-POLICY
 proposal IKEV2-PROPOSAL
```

### IKEv2 Keyring

#### R9_C1

```bash
crypto ikev2 keyring BRANCH-VPN
 peer R10_C2
  address 10.0.1.18
  pre-shared-key Golkey
```

#### R10_C2

```bash
crypto ikev2 keyring BRANCH-VPN
 peer R9_C1
  address 10.0.1.17
  pre-shared-key Golkey
```

### IKEv2 Profile

#### R9_C1

```bash
crypto ikev2 profile VPN-PROFILE
 match identity remote address 10.0.1.18 255.255.255.255
 identity local address 10.0.1.17
 authentication remote pre-share
 authentication local pre-share
 keyring local BRANCH-VPN
```

#### R10_C2

```bash
crypto ikev2 profile VPN-PROFILE
 match identity remote address 10.0.1.17 255.255.255.255
 identity local address 10.0.1.18
 authentication remote pre-share
 authentication local pre-share
 keyring local BRANCH-VPN
```

---

## Phase 2: IPsec Configuration

### IPsec Transform Set

```bash
crypto ipsec transform-set T-SET esp-aes 256 esp-sha256-hmac
 mode tunnel
```

### IPsec Profile

```bash
crypto ipsec profile IPSEC-PROFILE
 set transform-set T-SET
 set ikev2-profile VPN-PROFILE
```

---

## Tunnel Interface

### R9_C1

```bash
interface Tunnel0
 ip address 192.168.200.1 255.255.255.252
 tunnel source GigabitEthernet2
 tunnel destination 10.0.1.18
 tunnel protection ipsec profile IPSEC-PROFILE
```

### R10_C2

```bash
interface Tunnel0
 ip address 192.168.200.2 255.255.255.252
 tunnel source GigabitEthernet2
 tunnel destination 10.0.1.17
 tunnel protection ipsec profile IPSEC-PROFILE
```

---

## Static Routing Over Tunnel

### R9_C1

```bash
ip route 10.255.2.2 255.255.255.255 192.168.200.2
```

### R10_C2

```bash
ip route 10.255.2.1 255.255.255.255 192.168.200.1
```
