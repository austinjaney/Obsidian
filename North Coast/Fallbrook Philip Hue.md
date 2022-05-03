# Philips Hue MAC address
#northcoast #iot #FB 

Mac Address of Philips Hue Bridge for fallbrook campus
00:17:88:66:49:BE
We selected port 3/g43
3/g43  Gigabit - Level                 N/A     Unknown  Auto Down      Inactive

### Before Configuration
```
Fbk-CoreStack#show running-config interface ethernet 3/g43
switchport voice detect auto
description “USER/VOICE DUAL VLAN PORTS”
spanning-tree portfast
switchport mode general
switchport general pvid 500
switchport general allowed vlan add 500
switchport general allowed vlan add 502,2120 tagged
switchport general allowed vlan remove 1
voice vlan 502
```

### This port has been reconfigured to:
```
Fbk-CoreStack#show running-config interface ethernet 3/g43
switchport voice detect auto
description “USER/VOICE DUAL VLAN PORTS”
spanning-tree portfast
switchport mode general
switchport general pvid 2113
switchport general allowed vlan add 500
switchport general allowed vlan add 502 tagged
switchport general allowed vlan remove 1
voice vlan 502
```