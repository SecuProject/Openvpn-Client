# Openvpn Client 

## Firewall
### Install

```
apt install ufw
```

### Set IP address static
```
sudo nano /etc/network/interfaces
```

```bash
auto eth0
iface eth0 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
```

### UFW config - Block connection

```bash
ufw --force reset
ufw default deny incoming
ufw default deny outgoing
ufw allow in on tun0
ufw allow out on tun0
ufw allow in on eth0 from 192.168.1.100/24
ufw allow out on eth0 to 192.168.1.100/24
ufw allow out on eth0 to VPN_IP_ADDRESS port 1194 proto udp
ufw allow in on eth0 from VPN_IP_ADDRESS port 1194 proto udp
ufw enable
```

Check the status of UFW:

```
sudo ufw status
```

### Disabling IPv6

Edit file:
```
sudo nano /etc/sysctl.conf
```
```bash
net.ipv6.conf.all.disable_ipv6=1
net.ipv6.conf.default.disable_ipv6=1
net.ipv6.conf.lo.disable_ipv6=1
```

Apply changes:
```
sudo sysctl -p
```

## Check WAN IP Address
```bash
dig @resolver4.opendns.com myip.opendns.com +short -4
```

## Openvpn.conf
### DNS LEAK
```bash
script-security 2
up /etc/openvpn/update-resolv-conf
down /etc/openvpn/update-resolv-conf
```