# Notes – VRF-Lite Lab Adjustments and Considerations

This file documents changes, limitations, and key decisions made during the development of the VRF-Lite lab.

---

## Initial Plan (MPLS-Based)

* The original design included full MPLS Layer 3 VPN using:

  * `mpls ip`, LDP
  * VPNv4 address-family in BGP
  * Route-target import/export under `address-family ipv4 vrf`
  * Sham-link via Loopback10
  * BGP route-reflector

However, this setup failed due to platform limitations, mentioned below:

---

## CSR1000v Limitations

* `route-target` under `address-family ipv4 vrf` is **not supported** on CSR1000v
* `mpls ip` and LDP are **accepted** in config, but are **non-functional**
* `show bgp vpnv4 vrf` always returns **empty**, regardless of config
* CoPP policies (`control-plane service-policy`) **fail to install**

Because of this, the lab was converted from **MPLS L3VPN** to **VRF-Lite**.

---

## VRF-Lite Adjustments
* Moved `04_mpls.md` and `05_vrf_l3vpn.md` to `docs/`
* Created new file `05_vrf_lite.md`
* All per-VRF BGP now uses **manual redistribution** (no RT import/export)
* CE routers (R7, R8) use **OSPF only**, no BGP or MPLS
* Redistribution is now **one-way**: OSPF ↔ BGP inside VRFs on PE routers

---

## IKEv2 Note
* Attempted IKEv2 with `aes-gcm-256` and `integrity null`
* Although `aes-gcm-256` was supported, the `integrity null` was **rejected**
* Resolved by switching to `encryption aes-256` with `integrity sha256`

---

## ROAS + DHCP Relay Note

* R9 and R10 use subinterfaces (Router-on-a-Stick)
* DHCP relay (`ip helper-address`) must be placed on the subinterfaces

---

## Ubuntu Server

* Services hosted at `10.10.10.100` on VLAN 10
* Provides:

  * Syslog (rsyslog)
  * NTP (chrony)
  * SNMPv2/v3 (snmpd)
  * DHCP (isc-dhcp-server)

---

## BGP Route-reflector
* Moving RR to a larger and more complex BGP-focused lab.

---

## Testing and Verification

* All validation is done using:

  * `show ip route vrf CUSTOMER1`
  * `show ip protocols vrf CUSTOMER1`
  * `ping vrf CUSTOMER1 <destination>`
* No MPLS label tests are applicable in this version

---

## Summary

This project simulates a multi-site provider-style lab using pure VRF-Lite due to CSR1000v feature limitations. All original MPLS-related are removed and moved to `docs`, however, full functionality was maintained using routing separation, redistribution, and basic services.
