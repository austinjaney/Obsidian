# IDF 2 Switch
#northcoast #config #network 

```
console>enable

console#show running-config
!Current Configuration:
!System Description "PowerConnect 6224, 3.3.4.1, VxWorks 6.5"
!System Software Version 3.3.4.1
!Cut-through mode is configured as disabled
!
configure
vlan database
vlan 12-13,30-31,103,2012-2013,2091,2093
vlan routing 103 1
exit
stack
member 1 1
exit
ip address dhcp
ip routing
ip route 0.0.0.0 0.0.0.0 172.20.103.1
interface vlan 103
routing
ip address 172.20.103.10 255.255.255.0
exit
!
interface ethernet 1/g1
switchport mode general
switchport general pvid 2093
switchport general allowed vlan add 2093
switchport general allowed vlan add 12-13,2012-2013 tagged
exit
!
interface ethernet 1/g2
switchport access vlan 30
exit
!
interface ethernet 1/g3
switchport access vlan 30
exit
!
interface ethernet 1/g4
switchport access vlan 30
exit
!
interface ethernet 1/g5
switchport access vlan 30
exit
!
interface ethernet 1/g6
switchport access vlan 30
exit
!
interface ethernet 1/g21
switchport mode trunk
switchport trunk allowed vlan add 12-13,30-31,103,2012-2013,2093
exit
```