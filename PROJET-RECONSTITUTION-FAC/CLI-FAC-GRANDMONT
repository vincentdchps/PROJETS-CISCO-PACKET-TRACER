
---
``
## Switch L3 CORE BAT D 

création vlan :

```
enable
configure terminal
hostname COEUR_BAT_D
ip routing                 


vlan 10
 name ADMIN
vlan 20
 name TECHNIQUE
vlan 40
 name VOIP
vlan 50
 name ETUDIANTS
vlan 60
 name ETUDIANTS
exit
```

création des passerelles : 

```
! Interface Admin
interface vlan 10
 ip address 192.168.10.254 255.255.255.0
 no shutdown
 
 ! Interface Technique
interface vlan 20
 ip address 192.168.20.254 255.255.255.0
 no shutdown

! Interface Téléphonie
interface vlan 40
 ip address 192.168.40.254 255.255.255.0
 no shutdown

! Interface Etudiants Biblio 
interface vlan 50
 ip address 192.168.50.254 255.255.255.0
 no shutdown

! Interface Etudiants Bat L
interface vlan 60
 ip address 192.168.60.254 255.255.255.0
 no shutdown
```

configuration dhcp : 

```
ip dhcp excluded-address 192.168.10.254
ip dhcp excluded-address 192.168.40.254
ip dhcp excluded-address 192.168.50.254
ip dhcp excluded-address 192.168.60.254

ip dhcp pool POOL_ETUDIANTS
 network 192.168.60.0 255.255.255.0
 default-router 192.168.60.254
 dns-server 8.8.8.8

ip dhcp pool POOL_VOIP
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.254
 option 150 ip 192.168.40.254
 
 ip dhcp pool POOL_BIBLIOTHEQUE
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.254
 option 150 ip 192.168.50.254
 
 ip dhcp pool POOL_ADMIN
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.254
 option 150 ip 192.168.10.254
 
 ip dhcp pool POOL_TECHNIQUE
 network 192.168.20.0 255.255.255.0
 default-router 192.168.20.254
 option 150 ip 192.168.20.254


```


ip route :

```
interface GigabitEthernet0/1     
 no switchport                 
 ip address 192.168.100.1 255.255.255.0
 no shutdown
! Route par défaut vers internet
ip route 0.0.0.0 0.0.0.0 192.168.100.2
```


ACL : 

```
access-list 100 remark !!BLOQUER ETUDIANTS VERS ADMIN
access-list 100 deny ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 100 deny ip 192.168.60.0 0.0.0.255 192.168.10.0 0.0.0.255

access-list 100 remark !!BLOQUER ETUDIANTS VERS VOIP ---
access-list 100 deny ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
access-list 100 deny ip 192.168.60.0 0.0.0.255 192.168.40.0 0.0.0.255

access-list 100 remark !!AUTORISER INTERNET ET LE RESTE ---
access-list 100 permit ip any any
```


SVI (interface vlan virtuelle):

```
interface vlan 50
ip access-group 100 in
exit

interface vlan 60
ip access-group 100 in
exit
```

## Switch BAT L


```
hostname ACCES_BAT_L

vlan 60
 name ETUDIANTS


interface GigabitEthernet0/1
 switchport mode trunk


interface range FastEthernet0/1-24
 switchport mode access
 switchport access vlan 60
 exit

```

port security : 

```
interface range FastEthernet 0/1-24
 switchport mode access 
 switchport port-security 
 switchport port-security maximum 1 
 switchport port-security mac-address sticky  
 switchport port-security violation shutdown end write
```
## Switch Bat H

```
hostname ACCES_BAT_H

vlan 50
 name ETUDIANTS
vlan 10
 name ADMIN

interface GigabitEthernet0/1
 switchport mode trunk
 
interface range FastEthernet0/1
 switchport mode access
 switchport access vlan 50

!vlan pour l'imprimante (coté admin)
interface FastEthernet0/2
 switchport mode access
 switchport access vlan 10
```

port security : 

```
interface range FastEthernet 0/2-24
 switchport mode access 
 switchport port-security 
 switchport port-security maximum 1 
 switchport port-security mac-address sticky  
 switchport port-security violation shutdown end write
```
## Switch Bat G

```
hostname ACCES_BAT_G

vlan 10
 name ADMIN
vlan 40
 name VOIP
 
interface GigabitEthernet0/1
 switchport mode trunk

interface FastEthernet0/1
 switchport mode access
 switchport access vlan 10   
 switchport voice vlan 40    

```


## Routeur Internet 

```
hostname ROUTER_INTERNET

interface GigabitEthernet0/0
 ip address 192.168.100.2 255.255.255.0
 no shutdown
 
interface GigabitEthernet0/1
 ip address 8.8.8.254 255.255.255.0  
 no shutdown
 
ip route 192.168.0.0 255.255.0.0 192.168.100.1
```


NAT : 

```
interface GigabitEthernet 0/0
ip nat inside exit interface Serial 0/0/0 
ip nat inside exit interface GigabitEthernet 0/1
ip nat outside exit 
access-list 1 permit 192.168.0.0 0.0.255.255
!! ip publique pour appareils allant sur internet
ip nat pool PUBLIC_IPS 197.44.173.1 197.44.173.30 netmask 255.255.255.224
ip nat inside source list 1 pool PUBLIC_IPS overload 
```
