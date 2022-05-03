# Configure enable SSH on Cisco 3560-CX Switches
#sysadmin #network #cisco #northcoast #pv

From Nick Ostari
Make sure you write mem before doing the below in case you get locked out of the switch, if you get locked out you can just restart the switch and it will go back to the config before the below changes. Int vlan xx is your management VLAN, ip and gateway should be in the management vlan subnet. Thanks!

Configuring SSH on Cisco 3560-CX Switches
> Before doing this you should generate keys: “crypto key generate rsa”  
> Then choose “2048”  
```
clock timezone PST -8 0
clock summer-time PDT recurring
ip domain-name ncc.lan
login block-for 30 attempts 10 within 60
login delay 3
login on-failure log
login on-success log
ip ssh version 2
username nccadmin privilege 15 secret 0 nccadmin
enable password nccadmin
aaa new-model
aaa authentication login default local-case
aaa authentication enable default enable
aaa authorization exec default if-authenticated
aaa authorization network default if-authenticated
aaa session-id common
line vty 0 15
exec prompt timestamp
transport input ssh
int vlan 101
ip address 172.26.101.2 255.255.255.0
ip default-gateway 172.26.101.1
```

> Austins Thought: It looks like probably the way I need to do this is to create a VLAN that spans the whole network that is a management VLAN so we can get into the Cisco switches over that.   