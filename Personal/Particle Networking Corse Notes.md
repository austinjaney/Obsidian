# Particle Networking Corse Notes
#edu #personal #notes 

## Understanding Internet Connectivity
Layer 3 switches preform both switching and routing functionality

Ethernet frames contain Sources IP and Mac addresses and Destination IP and Mac addresses.  When A computer creates an ethernet frame the Source contains that computers IP address and MAC as well as the ultimate destinations IP address and the MAC of the default Gateway. 

ARP Requests ask “Who has this IP?”  So a computer might send one of these out when its trying to figure out what the Gateway MAC is.

## Network Troubleshooting Methodology
The OSI Model (Open Systems Interconnection) which has 7 layers
1. Physical
2. Data Link
3. Network
4. Transport
5. Session
6. Presentation
7. Application

### Find out if you have Layer 2 Connectivity
Ping the  broadcast address172.20.1.255 and check arp -a to see if you have a reply if you don’t have replies you have a layer 2 issue.

### Finding out if you have Layer 3 Connectivity

```
tracert google.com
```

Ping via ipv4
```
Ping -4 google.com
```

### NetSH
NetSH lets you edit windows settings in command prompt kind of like a network switch

Clear your arp table in windows
```
Arp -d
```

> NOTE: ARP resolves layer 3 (IP) addresses to layer 2 (MAC) addresses.  

### IPV6
There is no ARP in IPV6, only neighbor discovery.

