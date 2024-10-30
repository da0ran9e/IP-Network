# Lab2

## Single Router Inter-VLAN:

1. PC
- PC0
ip `192.168.5.2`
mask `255.255.255.0`
def `192.168.5.1`

- PC1
ip `192.168.7.2`
mask `255.255.255.0`
def `192.168.7.1`

2. SwitchA
```bash
enable
conf t
hostname SwitchA
enable secret class

enable password cisco

line vty 0 15
login
password cisco
exit

line console 0
login 
password cisco
exit

interface vlan1
ip address 192.168.1.2 255.255.255.0
exit

vlan 1 
name Native
vlan 10 
name Sales
vlan 20 
name Support
exit

conf t
interface fastethernet 0/5
switchport mode access
switchport access vlan 10

interface fastethernet 0/6
switchport mode access
switchport access vlan 10

interface fastethernet 0/7
switchport mode access
switchport access vlan 10

interface fastethernet 0/8
switchport mode access
switchport access vlan 10

end

conf t

interface fastethernet 0/9
switchport mode access
switchport access vlan 20

interface fastethernet 0/10
switchport mode access
switchport access vlan 20

interface fastethernet 0/11
switchport mode access
switchport access vlan 20

interface fastethernet 0/12
switchport mode access
switchport access vlan 20

end

show vlan



interface fastethernet0/1
switchport mode trunk
end 
```
3. RouterA
```bash
enable 
configure terminal 
hostname RouterA

enable secret class

enable password cisco

line vty 0 15
login
password cisco
exit

line console 0
login
password cisco
exit

interface fastethernet 0/0
no shutdown

interface fastethernet 0/0.1
encapsulation dot1q 1
ip address 192.168.1.1 255.255.255.0

interface fastethernet 0/0.2
encapsulation dot1q 10
ip address 192.168.5.1 255.255.255.0

interface fastethernet 0/0.3
encapsulation dot1q 20
ip address 192.168.7.1 255.255.255.0

end


```
## Inter-VLAN network with Single Router
1. Connect the Devices
2. Create VLANs in 1st floor

```bash
enable
configure terminal
vlan 1
name Native
exit
vlan 2
name Sales
exit
vlan 3
name HR
exit
vlan 4
name ENG
exit
```
3. Set the 1st Floor Switch as VTP Server for CORP domain
```bash
vtp domain CORP
vtp mode server

```
4. Configure other switches
```bash
enable
configure terminal
vtp domain CORP
vtp mode client

```
5. Assign VLANs to Ports
```bash
interface FastEthernet0/1
switchport mode access
switchport access vlan 2

```
6. configure the router
```bash
enable
configure terminal
interface GigabitEthernet0/0
no shutdown

interface GigabitEthernet0/0.1
encapsulation dot1Q 1
ip address 192.168.1.1 255.255.255.0

interface GigabitEthernet0/0.2
encapsulation dot1Q 2
ip address 192.168.2.1 255.255.255.0

interface GigabitEthernet0/0.3
encapsulation dot1Q 3
ip address 192.168.3.1 255.255.255.0

interface GigabitEthernet0/0.4
encapsulation dot1Q 4
ip address 192.168.4.1 255.255.255.0

```
## Virtual LAN - Inter-VLAN with Switch Layer 3

1. 3560
```bash
enable 
conf t
hostname 3550A
ip routing 

interface fa0/1
switchport trunk encapsulation dot1q
switchport mode trunk

interface fa0/2
switchport trunk encapsulation dot1q
switchport mode trunk

interface fa0/3
switchport trunk encapsulation dot1q
switchport mode trunk

interface fa0/4
switchport trunk encapsulation dot1q
switchport mode trunk

interface vlan 1
ip address 192.168.10.1 255.255.255.0
no shut

exit
vlan 10
vlan 20
exit
exit

sh vlan
conf t
vtp domain VLANLab
exit

sh vtp status
```

2. 2950A

``` bash
conf t 
hostname 2950A
interface fa0/11
switch mode trunk 

interface fa0/12
switch mode trunk

interface vlan 1
ip address 192.168.10.2 255.255.255.0
no shutdown
exit
```

3. 2950B
```bash
conf t 
hostname 2950B
interface fa0/11
switch mode trunk 

interface fa0/12
switch mode trunk

interface vlan 1
ip address 192.168.10.3 255.255.255.0
no shutdown
exit
```

4. 2950A/B
```bash
vtp domain VLANLab
vtp mode client 
exit 

show vtp status
show vlan
```
5. 2950A
```bash
conf t
int fa0/2
switchport access vlan 10
exit
```
6. 2950B
```bash
conf t
int fa0/2
switchport access vlan 20
exit
```
7. 2950A
```bash
conf t
int vlan 10
ip address 10.10.10.1 255.255.255.0
int vlan 20
ip address 20.20.20.1 255.255.255.0
exit
exit
ping 192.168.10.2
ping 192.168.10.3
```

## Inter-VLAN network with Switch Layer 3
