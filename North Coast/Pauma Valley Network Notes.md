# Pauma Valley Network Notes
#northcoast #cisco #pv #notes 

## Switches
|Switch					|ip address					|
|:------ |:------ |
|Network Rack Core 6248  |ssh://nccadmin@172.26.101.1 |
|Marylyn’s Office Cisco 	|ssh://nccadmin@172.26.101.2 |

### Power-connect trunk port -> Cisco 3650-CX
```
description "VLAN-TRUNK-TO-OFFICE"
switchport mode trunk
switchport trunk allowed vlan add 10-11,2612-2614,2691
```

### Power-connect AP Port
```
PaumaValley-CoreStack#show running-config interface ethernet 1/g42
description "Cisco AP Outside Server Room"
spanning-tree portfast
switchport mode general
switchport general pvid 2691
switchport general allowed vlan add 2691
switchport general allowed vlan add 2612-2613 tagged
```

### Cisco 3650-CX trunk port -> Power-connect
> Note: before adding Vlan ID’s higher than 1001 you must run: “vtp mode transparent”  
```
interface GigabitEthernet1/0/1
switchport trunk allowed vlan 10,11,2612-2614,2691
switchport mode trunk
```

### AP Port on the Cisco 3650-CX
```
interface GigabitEthernet1/0/2
 switchport trunk allowed vlan 2612-2614,2691
 switchport trunk native vlan 2691
 switchport mode trunk
 spanning-tree portfast edge trunk
```

### Data / Voice Ports
```
interface GigabitEthernet1/0/3
 switchport access vlan 10
 switchport mode access
 switchport voice vlan 11
 srr-queue bandwidth share 1 30 35 5
 priority-queue out 
 no cdp enable
 mls qos trust cos
 auto qos trust 
 spanning-tree portfast edge
end
```

### Xerox Printer Port
```
switchport trunk allowed vlan 2612,10
switchport trunk native vlan 2612
switchport mode trunk
```

### Powerconnect Core PV Vlans Overview:
```
VLAN       Name                         Ports          Type      Authorization
-----  ---------------                  -------------  -----     -------------
1      Default                          ch1-48,        Default   Required     
                                        1/g44-1/g46,
                                        1/g48-1/xg4
10     User-VLAN                        1/g2-1/g43     Static    Required     
11     Voice                            1/g2-1/g43     Static    Required     
65     Security Cameras                 1/g43          Static    Required     
98     Sonicwall VLAN - Native VLAN     1/g43          Static    Required     
99     Connection-To-Firewall           1/g43,1/g48    Static    Required     
100    Server-VLAN                      1/g43          Static    Required     
2612   CISCO PRIVATE WIRELESS VLAN      1/g43-1/g44    Static    Required     
2613   CISCO GUEST WIRELESS VLAN        1/g1,          Static    Required     
                                        1/g43-1/g44
2614   Pauma IOT Wireless               1/g43          Static    Required     
2691   Cisco Wireless AP VLAN           1/g43-1/g44,   Static    Required     
                                        1/g47-1/g48
```

### Cisco Data/Voice Ports
```
interface GigabitEthernet1/0/3
 switchport access vlan 10
 switchport mode access
 switchport voice vlan 11
 srr-queue bandwidth share 1 30 35 5
 priority-queue out 
 no cdp enable
 mls qos trust cos
 auto qos trust 
 spanning-tree portfast edge
end
```