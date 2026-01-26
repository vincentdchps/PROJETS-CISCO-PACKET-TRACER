# CLI-AZA-DMZ

## ASA 5505

```
enable
conf t
activation-key 469a4734 5017c667 3624c935 90515635 2e389531
exit
wr
reload

enable
conf t
hostname FIREWALL-ASA

interface vlan 1
 nameif INSIDE
 security-level 100
 ip address 192.168.10.1 255.255.255.0
 no shut
 exit

interface vlan 2
 nameif OUTSIDE
 security-level 0
 ip address 203.0.113.2 255.255.255.252
 no shut
 exit

interface vlan 3
 nameif DMZ
 security-level 50
 ip address 172.16.50.1 255.255.255.0
 no shut
 exit


interface Ethernet0/0
 switchport access vlan 2
 no shut
 exit

interface Ethernet0/1
 switchport access vlan 1
 no shut
 exit

interface Ethernet0/2
 switchport access vlan 3
 no shut
 exit
route OUTSIDE 0.0.0.0 0.0.0.0 203.0.113.1
```
