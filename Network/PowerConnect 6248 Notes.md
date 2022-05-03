# Dell Powerconnect 6248 Notes
#network #hardware

Show all ip addresses and Mac addresses

```
Show arp
```

Show all Mac Addresses and Ports

```
Show bridge address-table
```

After reset switches take the IP 192.168.2.1/24
To connect set your IP to 192.168.2.2/24

Renumber switches: running this command will cause the switch to reboot. 

```
console(config)# switch <existing ID> renumber <new ID>
```

```
console(config)# switch 2 renumber 1
```

## Setup
By default putting a dell switch into managment mode gives the switch the ip address of 192.168.2.1

Configuring your system with the ip of 192.168.2.2 and subnet 255.255.255.0 will allow you to access the web ui and ssh into the switch.

The default login for dell switches is admin/admin