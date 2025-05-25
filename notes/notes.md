# Notes – VRF-Lite Lab Adjustments and Considerations

This file documents changes, limitations, and key decisions made during the development of the VRF-Lite lab.

---

## Initial Plan (MPLS-Based)

* The original design included full MPLS Layer 3 VPN using:

  * `mpls ip`, LDP
  * VPNv4 address-family in BGP
  * Route-target import/export under `address-family ipv4 vrf`
  * Sham-link via Loopback10

However, this setup failed due to platform limitations, mentioned below:

---

## CSR1000v Limitations

* `route-target` under `address-family ipv4 vrf` does **not exist** on CSR1000v
* `mpls ip` and LDP are **accepted** but non-functional
* `show bgp vpnv4 vrf` remains empty regardless of config
* CoPP policies (`control-plane service-policy`) fail to install

Because of this, the lab was converted to **VRF-Lite** instead of full MPLS L3VPN.

---

## VRF-Lite Adjustments
* Removed `04_mpls.md` from the project, to `docs`
* Removed `05_vrf_l3vpn.md` from the project, to `docs`
* Created `05_vrf_lite.md` to the project
* All BGP per-VRF now uses simple redistribution (no route-targets)
* CE routers (R7, R8) use OSPF only, no BGP or MPLS
* Redistribution is now one-way: OSPF ↔ BGP inside the VRF (on PE)

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

## Testing and Verification

* All validation is done using:

  * `show ip route vrf CUSTOMER1`
  * `show ip protocols vrf CUSTOMER1`
  * `ping vrf CUSTOMER1 <destination>`
* No MPLS label tests are applicable in this version

---

## Summary

This project simulates a multi-site provider-style lab using pure VRF-Lite due to CSR1000v feature limitations. All original MPLS-related steps are removed and moved to `docs`, however, full functionality was maintained using routing separation, redistribution, and basic services.
