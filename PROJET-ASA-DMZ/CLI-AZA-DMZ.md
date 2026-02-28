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


## Router FAI-ISP

```
enable
conf t
hostname FAI-ISP

interface g0/0
 ip address 203.0.113.1 255.255.255.252
 no shut
 exit

interface g0/1
 ip address 8.8.8.1 255.255.255.0
 no shut
 exit
```


## Switch 2960 Employes 

```cisco
enable
conf t
hostname SW-INSIDE-SECURE

vlan 10
 name EMPLOYES
 exit

interface g0/1
 switchport mode access
 switchport access vlan 10
 ip dhcp snooping trust
 exit


interface range f0/1-24
 switchport mode access
 switchport access vlan 10

 switchport port-security
 switchport port-security maximum 1
 switchport port-security violation shutdown
 switchport port-security mac-address sticky

 no ip dhcp snooping trust
 
 spanning-tree bpduguard enable
 exit

ip dhcp snooping
ip dhcp snooping vlan 10
```

