## RIP V.1
1. Router:
```bash
enable 
erase startup-config
```
```bash
reload
```
2. Router0:
```bash
enable
configure terminal
hostname GAD

interface GigabitEthernet0/0/0
ip address 172.16.0.1 255.255.0.0
no shutdown
exit

interface Serial0/1/0
ip address 172.17.0.1 255.255.0.0
clock rate 64000      
no shutdown
exit

enable secret class
line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

end
write memory

router rip
network 172.16.0.0
network 172.17.0.0
exit
exit

```
3. Router1:
```bash
enable
configure terminal
hostname BHM

interface GigabitEthernet0/0/0
ip address 172.18.0.1 255.255.0.0
no shutdown
exit

interface Serial0/1/0
ip address 172.17.0.2 255.255.0.0
clock rate 64000      
no shutdown
exit

enable secret class
line console 0
password cisco
login
exit

line vty 0 4
password cisco
login
exit

end
write memory

router rip
network 172.17.0.0
network 172.18.0.0
exit
exit

```
4. Router
```bash
sh ip interface brief
sh ip route
sh ip protocol

debug ip rip
debug ip rip events
debug ip rip trigger
debug ip rip database
```
5. Router0:
```bash
conf t
router rip
passive-interface serial0/0
debug ip rip events
```

## RIP V.2
6. Router
```bash
conf t
router rip
version 2
exit

debug ip rip
debug ip packet
```
## OSPF
1. Router1
```bash
enable
configure terminal
hostname R1

interface GigabitEthernet0/0/0
ip address 172.16.1.17 255.255.255.240
no shutdown
exit

interface Serial0/1/0
ip address 192.168.10.1 255.255.255.252    
no shutdown
exit

interface Serial0/2/0
ip address 192.168.10.5 255.255.255.252
clock rate 64000     
no shutdown
exit

router ospf 1
network 172.16.1.16 0.0.0.15 area 0
network 192.168.10.0 0.0.0.3 area 0
network 192.168.10.4 0.0.0.3 area 0
end

int loopback 0
ip add 10.1.1.1 255.255.255.255

router ospf 1
router-id 10.4.4.4 

end 
clear ip ospf process

router ospf 1
no router-id 10.4.4.4
end
clear ip ospf process

int serial0/1/0
bandwidth 64
int serial0/2/0
bandwidth 64

interface loopback1
ip add 172.30.1.1 255.255.255.255 
ip route 0.0.0.0 0.0.0.0 loopback1

router ospf 1
default-information originate

```

2. Router2
```bash
enable
configure terminal
hostname R2

interface GigabitEthernet0/0/0
ip address 10.10.10.1 255.255.255.0
no shutdown
exit

interface Serial0/1/0
ip address 192.168.10.2 255.255.255.252    
no shutdown
exit

interface Serial0/2/0
ip address 192.168.10.9 255.255.255.252  
clock rate 64000   
no shutdown
exit

router ospf 1
network 10.10.10.0 0.0.0.255 area 0
network 192.168.10.0 0.0.0.3 area 0
network 192.168.10.8 0.0.0.3 area 0
end

int loopback 0
ip add 10.2.2.2 255.255.255.255

int serial0/1/0
bandwidth 64
int serial0/2/0
bandwidth 64
```

3. Router3
```bash
enable
configure terminal
hostname R3

interface GigabitEthernet0/0/0
ip address 172.16.1.33 255.255.255.248
no shutdown
exit

interface Serial0/1/0
ip address 192.168.10.6 255.255.255.252    
no shutdown
exit

interface Serial0/2/0
ip address 192.168.10.10 255.255.255.252  
clock rate 64000   
no shutdown
exit

router ospf 1
network 172.16.1.32 0.0.0.7 area 0
network 192.168.10.4 0.0.0.3 area 0
network 192.168.10.8 0.0.0.3 area 0
end

int loopback 0
ip add 10.3.3.3 255.255.255.255

interface serial0/1/0
ip ospf cost 1562 
interface serial0/2/0
ip ospf cost 1562 
```

```bash
sh ip protocols
sh ip ospf
sh ip ospf interfaces
sh ip ospf neighbor
sh ip route
sh interfaces serial0/1/0
sh ip ospf interface 

```

## RIP, OSPF
- 3 switches:
Fa0/1 Switch0 to Fa0/2 Switch1
Fa0/1 Switch1 to Fa0/2 Switch2
Fa0/1 Switch2 to Fa0/2 Switch0 
- 4 Routers:
Gig0/0 Router0 to Fa0/10 Switch0
Gig0/0 Router1 to Fa0/11 Switch0
Gig0/0 Router2 to Fa0/10 Switch1
Gig0/0 Router3 to Fa0/11 Switch1

1. Router:
```bash
enable
configure terminal
hostname Router0
interface GigabitEthernet0/0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.10.0
network 192.168.11.0
network 192.168.12.0
network 192.168.13.0
exit

```

```bash
enable
configure terminal
hostname Router1
interface GigabitEthernet0/0/0
ip address 192.168.11.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.10.0
network 192.168.11.0
network 192.168.12.0
network 192.168.13.0
exit

```
```bash
enable
configure terminal
hostname Router2
interface GigabitEthernet0/0/0
ip address 192.168.12.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.10.0
network 192.168.11.0
network 192.168.12.0
network 192.168.13.0
exit

```

```bash
enable
configure terminal
hostname Router3
interface GigabitEthernet0/0/0
ip address 192.168.13.1 255.255.255.0
no shutdown
exit
router rip
version 2
network 192.168.10.0
network 192.168.11.0
network 192.168.12.0
network 192.168.13.0
exit
```
```bash
enable
configure terminal
interface FastEthernet0/1
switchport mode trunk
no shutdown
exit
interface FastEthernet0/2
switchport mode trunk
no shutdown
exit
interface FastEthernet0/10
switchport mode access
no shutdown
exit
interface FastEthernet0/11
switchport mode access
no shutdown
exit

```