# Документация RED OS по настройке DHCP сервера

sudo dnf upgrade --refresh

nmcli con show
nmcli con mod "Проводное подключение 1" con-name enp7s0

dnf install dhcp-server

```
subnet 10.10.10.0 netmask 255.255.255.0 {
   range 10.10.10.3 10.10.10.100;
   range 10.10.10.150 10.10.10.200;
   option domain-name-servers 77.88.8.88, 77.88.8.2;
   option routers 10.10.10.1;
   option broadcast-address 10.10.10.255;
   default-lease-time 600;
   max-lease-time 7200;
 }

 ```

Файл /etc/sysconfig/dhcpd

```
DHCPDARGS=enp7s0
```

Файл /etc/sysconfig/network-scripts/ifcfg-enp7s0

```
TYPE="Ethernet"
BOOTPROTO="none"
DNS1="10.10.10.1"
IPADDR0="10.10.10.1"
PREFIX0=24
GATEWAY0=10.10.10.1
DEFROUTE="yes"
PEERDNS="yes"
PEERROUTES="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_PEERDNS="yes"
IPV6_PEERROUTES="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="enp7s0"
DEVICE="enp7s0"
ON BOOT="yes"
```
systemctl enable dhcpd
systemctl start dhcpd
