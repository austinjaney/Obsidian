# Cisco Catalyst 3560-CX
#sysadmin #northcoast #cisco 

## Things We don’t know how to do yet
- [x] Add nccadmin
- [x] Enable SSH
```
config t
hostname PVOFFICE
ip domain-name PVOFFICE
crypto key generate rsa
end

show ip ssh
copy run start
```

- [ ] Configure Switch IP address
```
config t
interface vlan 101
ip address 172.26.101.2 255.255.255.0
```

- [ ] Tag additional Vlans

# Test Switch
172.26.1.4

aaa new-model
aaa authentication login default local
aaa authorization exec local
aaa authorization exec default local

## Note from Nick 
Hi Austin,
Its just like you did for the AP, pvid in Cisco world is switchport trunk native vlan.
```
switchport trunk allowed vlan 2612-2614,2691
 switchport trunk native vlan 2691
 switchport mode trunk
```

Now if you are doing it for combo VoIP/workstation ports, you can do something like the below using access and voice vlan:
```
interface GigabitEthernet1/0/18
switchport access vlan 254
switchport mode access
switchport voice vlan 54
srr-queue bandwidth share 1 30 35 5
priority-queue out
 no cdp enable
mls qos trust cos
auto qos trust
 spanning-tree portfast edge
```