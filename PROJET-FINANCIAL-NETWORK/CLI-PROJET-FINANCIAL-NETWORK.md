# CLI-PROJET-FINANCIAL-NETWORK

## Switch 2960 HR-SW

```
--configuration de base
en
conf t

hostname HR-SW

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

--vlan
vlan 10
name DATA
vlan 120
name VOICE
exit

int range fa0/1-2
switchport mode trunk
exit

int range fa0/3-24
switchport mode access
switchport access vlan 10
switchport voice vlan 120
exit

do wr
```

## Switch 2960 CS-SW

```
en
conf t

hostname CS-SW

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

--vlan
vlan 20
name DATA
vlan 120
name VOICE
exit

int range fa0/1-2
switchport mode trunk
exit

int range fa0/3-24
switchport mode access
switchport access vlan 20
switchport voice vlan 120
exit


do wr
```

## Switch 2960 MK-SW

```
en
conf t

hostname MK-SW

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

--vlan
vlan 30
name DATA
vlan 120
name VOICE
exit

int range fa0/1-2
switchport mode trunk
exit

int range fa0/3-24
switchport mode access
switchport access vlan 30
switchport voice vlan 120
exit


do wr
```


## Switch 2960 LM-SW

```
en
conf t

hostname LM-SW

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

--vlan
vlan 40
name DATA
vlan 120
name VOICE
exit

int range fa0/1-2
switchport mode trunk
exit

int range fa0/3-24
switchport mode access
switchport access vlan 40
switchport voice vlan 120
exit


do wr
```

## Switch 2960 IT-SW

```
en
conf t

hostname IT-SW

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

--vlan
vlan 50
name DATA
vlan 120
name VOICE
exit

int range fa0/1-2
switchport mode trunk
exit

int range fa0/3-24
switchport mode access
switchport access vlan 50
switchport voice vlan 120
exit


do wr
```

## Switch 2960 SERVER-SIDE SW 

```
en
conf t

hostname SERVER-SIDE-SW

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

do wr
```

## Switch Layer 3 3650 HQ-MSLW1

```
en
conf t

hostname HQ-MSLW1

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

username cisco password cisco
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh version 2
line vty 0 15
login local
transport input ssh
exit
--vlan 
int range gig1/0/2-7
switchport mode trunk
exit

vlan 10
name HR
vlan 20
name CS
vlan 30
name MK
vlan 40
name LM
vlan 50
name IT
vlan 120
name VOICE
exit
do wr
```
## Switch Layer 3 3650 HQ-MSLW2

```
en
conf t

hostname HQ-MSLW2

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

username cisco password cisco
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh version 2
line vty 0 15
login local
transport input ssh
exit

--vlan
int range gig1/0/2-6
switchport mode trunk
exit

vlan 10
name HR
vlan 20
name CS
vlan 30
name MK
vlan 40
name LM
vlan 50
name IT
vlan 120
name VOICE
exit


do wr
```


## Router 2911 HQ-Router

```
en
conf t

hostname HQ-Router

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

username cisco password cisco
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh version 2
line vty 0 15
login local
transport input ssh
exit

do wr
```

## Router 2911 Server-Side-Router

```
en
conf t

hostname Server-Side-Router

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

username cisco password cisco
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh version 2
line vty 0 15
login local
transport input ssh
exit

int range fa0/2-5
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown
int range fa0/6-24, gig 0/1-2
switchport mode access
switchport access vlan 99
shutdown
exit
do wr
```


## Router 2911 Safaricorm 

```
en
conf t

hostname Safaricorm

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

username cisco password cisco
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh version 2
line vty 0 15
login local
transport input ssh
exit

do wr
```


## Router 2911 JTL-IS

```
en
conf t

hostname JTL-ISP

enable password cisco

line console 0
password cisco 
login
exit

banner motd #NO UNAUTHORISED ACCESS!!#

no ip domain lookup 

service password-encryption

username cisco password cisco
ip domain-name cisco.com
crypto key generate rsa 
1024
ip ssh version 2
line vty 0 15
login local
transport input ssh
exit

do wr
```


