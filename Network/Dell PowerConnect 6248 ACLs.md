# Proposed ACLs for Vista Core Switches
#dell #network 

> The combination of these 3 rules should isolate guest so that it can not talk with any of our current (or future) infrastructure. A possible addition to this may be to create additional rules that cover the 10.x.x.x and 192.168.x.x areas.

```bash
access-list VLAN2013 permit udp 10.20.16.0 0.0.15.255 eq 67 172.20.1.101 0.0.0.3
```

The Deny All guest wireless traffic rule.

```bash
access-list VLAN2013 deny ip 10.20.16.0 0.0.15.255 172.16.0.0 0.15.255.255
```

The Allow All traffic rule for hosts that no rules apply to

```bash
access-list VLAN2013 permit ip any any
```

Copy Pasta working isolation at vista

```bash
access-list VLAN2013 permit udp 10.20.16.0 0.0.15.255 eq 67 172.20.1.101 0.0.0.3
access-list VLAN2013 deny ip 10.20.16.0 0.0.15.255 172.16.0.0 0.15.255.255
access-list VLAN2013 permit ip any any
interface vlan 2013
ip access-group VLAN2013 in
```
> adapting this ruleset to other campuses can be done by customizing their guest wireless ip information and corectly zoning it, since this rule set blocks the entire 172.16-32.x.x space no further modifications are needed.

A further modification that could be made would be to drop all traffic from the entire 10.x.x.x network instead of the sepcified guest network traffic:

```bash
access-list VLAN2013 deny ip 10.0.0.0 0.255.255.255 172.16.0.0 0.15.255.255
```
> Using this approach casts a wide net that would include the guest network across all future campuses assuming we stay to the model of guest always being in the 10.x.x.x block. A rule for our server vlan that drops traffic coming from the guest vlan would probably be useful to close the other end so that even if an off site campus is configured incorrectly or with different equipment traffic from a 10.x.x.x address would always be dropped.

## Notes
Dell notes that you should have ACLs for both ends of your network paths:

>"In order to minimize unnecessary traffic across a network, it is best to apply an inbound ACL as close to the source IP as possible and an outbound ACL closest to the destination IP. One should consider this carefully, as it is generally not recommended to carry packets across a network only to have them dropped, when they could have been dropped on the first hop."

I think what this means is that the methodology that we need to implement is to drop packets at the edge as well as enforce dropping packets on the interior of our network.

So in other words in our guest network we should have an ACL applied to the guest clan that reads: “Drop all packets inbound towards the LAN (172.16-32.x.x)”

And another rule on the server VLAN that reads: “Drop all packets coming from Guest (10.16.20.x)”