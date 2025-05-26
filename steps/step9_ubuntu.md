# Step 10 – Ubuntu Setup (Syslog, NTP, DHCP, SNMP)

This step is performed entirely on the Ubuntu VM. Connect Ubuntu to VLAN 10 and configure the following services.

---

## Network Setup

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

```yaml
network:
  version: 2
  ethernets:
    ens3:
      dhcp4: no
      addresses:
        - 10.10.10.100/24
      gateway4: 10.10.10.254
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Then apply:

```bash
sudo netplan apply
```

---

## Syslog Server (rsyslog)

```bash
sudo apt update && sudo apt install rsyslog -y
sudo nano /etc/rsyslog.conf
```

Remove:

```bash
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")
```

```bash
sudo systemctl restart rsyslog             # Requires restart
```

Logs will go to `/var/log/syslog`

---

## NTP Server

```bash
sudo apt install chrony -y
sudo nano /etc/chrony/chrony.conf
```

Edit:

```bash
allow 10.10.10.0/24
```

```bash
sudo systemctl restart chrony              # Requires restart
```

---

## SNMP Receiver

```bash
sudo apt install snmp snmpd -y
sudo nano /etc/snmp/snmpd.conf
```

Set:

```bash
rocommunity public
sysLocation Ubuntu
sysContact admin@golrang.local
```

Restart:

```bash
sudo systemctl restart snmpd
```

---

## DHCP Server

```bash
sudo apt install isc-dhcp-server -y
sudo nano /etc/dhcp/dhcpd.conf
```

```bash
subnet 10.10.10.0 netmask 255.255.255.0 {
  range 10.10.10.150 10.10.10.200;
  option routers 10.10.10.254;
  option domain-name-servers 8.8.8.8;
}
```

```bash
sudo systemctl restart isc-dhcp-server    # Requires restart
```
