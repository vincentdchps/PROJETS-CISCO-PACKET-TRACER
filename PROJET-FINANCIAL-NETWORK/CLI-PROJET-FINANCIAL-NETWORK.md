# CLI-PROJET-FINANCIAL-NETWORK

## Switch 2960 HR-SW

```
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

do wr
```


## Router 2911 Safaricorm 

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

do wr
```

