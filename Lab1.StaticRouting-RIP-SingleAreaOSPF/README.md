# Lab1

## Router0
```
enable
configure terminal
hostname Boston

interface gigabitEthernet 0/0/0
ip address 172.16.10.1 255.255.255.0
no shutdown
exit

interface serial 0/1/0 
ip address 172.16.20.1 255.255.255.0
no shutdown
exit
```
## Router1
```
enable 
configure terminal 
hostname Buffallo

interface gigabitEthernet 0/0/0
ip address 172.16.30.1 255.255.255.0
no shutdown
exit

interface serial 0/1/0
ip address 172.16.20.2 255.255.255.0
no shutdown
exit 

interface serial 0/2/0
ip address 172.16.40.1 255.255.255.0
no shutdown
exit
```
## Router2
```
enable 
configure terminal 
hostname Bangor

interface gigabitEthernet 0/0/0
ip address 172.16.50.1 255.255.255.0
no shutdown
exit

interface serial 0/2/0
ip address 172.16.40.2 255.255.255.0
no shutdown
exit 
```
## PC
### pc0
ipv4 `172.16.10.10`
mask `255.255.255.0`
default gateway `172.16.10.1`

### pc2
ipv4 `172.16.30.10`
mask `255.255.255.0`
default gateway `172.16.30.1`

### pc3
ipv4 `172.16.50.10`
mask `255.255.255.0`
default gateway `172.16.50.1`

## Static routing
### Boston
```
conf t
ip route 172.16.30.0 255.255.255.0 172.16.20.2
ip route 172.16.40.0 255.255.255.0 172.16.20.2
ip route 172.16.50.0 255.255.255.0 172.16.20.2
exit
show ip route
```
### Buffallo
```
conf t
ip route 172.16.10.0 255.255.255.0 172.16.20.1
ip route 172.16.50.0 255.255.255.0 172.16.40.2
exit
show ip route
```
### Bangor
```
conf t
ip route 172.16.10.0 255.255.255.0 172.16.40.1
ip route 172.16.20.0 255.255.255.0 172.16.40.1
ip route 172.16.30.0 255.255.255.0 172.16.40.1
exit
show ip route
```
## Remove static routing
### Boston
```
conf t
no ip route 172.16.30.0 255.255.255.0
no ip route 172.16.40.0 255.255.255.0
no ip route 172.16.50.0 255.255.255.0
exit
show ip route
```
### Buffallo
```
conf t
no ip route 172.16.10.0 255.255.255.0
no ip route 172.16.50.0 255.255.255.0
exit
show ip route
```
### Bangor
```
conf t
no ip route 172.16.10.0 255.255.255.0
no ip route 172.16.20.0 255.255.255.0
no ip route 172.16.30.0 255.255.255.0
exit
show ip route
```
## RIP
### Boston
```
conf t
router rip
version 2 
network 172.16.10.0
network 172.16.20.0
network 172.16.30.0
exit
exit
show ip rip database
```
### Buffallo
```
conf t
router rip
version 2 
network 172.16.10.0
network 172.16.50.0
exit
exit
show ip rip database
```
### Bangor
```
conf t
router rip
version 2 
network 172.16.10.0
network 172.16.20.0
network 172.16.30.0
exit
exit
show ip rip database
```
## Remove RIP
### Boston
```
conf t
router rip
no network 172.16.30.0
no network 172.16.40.0
no network 172.16.50.0
exit
exit
show ip rip database
```
### Buffallo
```
conf t
router rip
no network 172.16.10.0
no network 172.16.50.0
exit
exit
show ip rip database
```
### Bangor
```
conf t
router rip
no network 172.16.10.0
no network 172.16.20.0
no network 172.16.30.0
exit
exit
show ip rip database
```
## Single OSPF
```
conf t
router ospf 123
network 172.16.10.0 0.0.0.255 area 0
network 172.16.20.0 0.0.0.255 area 0
network 172.16.30.0 0.0.0.255 area 0
network 172.16.40.0 0.0.0.255 area 0
network 172.16.50.0 0.0.0.255 area 0
```