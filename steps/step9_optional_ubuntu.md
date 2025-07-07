# Step 9 - OPTIONAL Ubuntu Setup (Syslog, NTP, DHCP, SNMP)

This step is performed entirely on the Ubuntu VM. Connect the Ubuntu machine to VLAN 10 and configure the following services:

- Syslog (rsyslog)
- NTP server (chrony or ntpd)
- DHCP server (isc-dhcp-server)
- **Optional** - SNMP (snmpd)

* Do note that my Linux knowledge is limited, if this doesn’t work properly, just skip it. **This step is optional.**

---

## Network Setup

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 10.10.10.100/24
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
      routes:
        - to: 0.0.0.0/0
          via: 10.10.10.254
```

Then apply:

```bash
# Move the file
sudo mv /etc/netplan/01-network-manager-all.yaml /etc/netplan/01-network-manager-all.yaml.bak

# Fixes your permissions:
sudo chmod 600 /etc/netplan/01-netcfg.yaml

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
# It is not active, however, Ubuntu became buggy on my end, best to remove.
module(load="imudp")
input(type="imudp" port="514")
module(load="imtcp")
input(type="imtcp" port="514")
```

```bash
# Requires restart
sudo systemctl restart rsyslog
```

Logs will go to `/var/log/syslog`

---

## NTP Server

```bash
sudo apt install chrony -y
sudo nano /etc/chrony/chrony.conf
```

Add:

```bash
allow 10.10.10.0/24
```

```bash
# Requires restart
sudo systemctl restart chrony
```

---

## SNMP Receiver - Not required due to Router limitations

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

```bash
# Requires restart
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
# Requires restart
sudo systemctl restart isc-dhcp-server
```
