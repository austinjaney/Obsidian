# SME Server Replacement
#northcoast

Ethernet Ports for new Servers on Core-switch 1/g27,28,29,30
```
SME-CoreStack#show running-config interface ethernet 1/g27
description "Server Ports"
spanning-tree portfast
switchport access vlan 401
```

IP Addresses for New Servers
SHV1: 172.23.1.104
SHV2: 172.23.1.105

## Services Being Replaced
### Hardware
- [ ] Give IP Addresses to Hyper-v Servers
- [ ] Install PowerChute for local APC
### Radius
- [ ] Add SRS2 to the WLC with new shared secret (needs to be less than 12 char)
### Printing
- [ ] Undeploy Printers from Group Policy “(C) SME Printers”
### DHCP
- [ ] Install DHCP Service and import DHCP on DCS1A
- [ ] Look at DHCP Helper addresses on switches
### DNS
- [ ] Check DNS Functionality and look at switch configurations
### Domain Services
- [ ] Move Domain Controllers into the correct sites
- [ ] Check Replication
- [ ] Demote Old Domain Controllers

## Testing
- [ ][ ] Run gpupdate /force on all the computers