# Daily Note April 1 2020
#day

Scratch

description "Cisco AP IT Trailer"
spanning-tree portfast
switchport mode general
switchport general pvid 2096
switchport general allowed vlan add 2096
switchport general allowed vlan add 5,12-13,53,2012-2013,2015-2016 tagged
switchport general allowed vlan add 3141 tagged
