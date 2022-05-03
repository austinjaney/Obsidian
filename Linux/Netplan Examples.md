# Netplan Examples
#linux #ubuntu #sysadmin 

Working YAML for Netplan

```bash
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      dhcp6: no
      addresses: [172.16.1.6/24]
      gateway4: 172.16.1.1
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
```

Some Examples for changing the MTU of an adapter in Netplan.

```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    # Configure MTU
    bigmtu1:
      match:
        macaddress: 00:e0:8d:7e:1f:37
      mtu: "9000"
```

```bash
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: no
      dhcp6: no
      tmu: 1472
      addresses: [209.242.155.200/26]
      gateway4: 209.242.155.193
      nameservers:
        addresses: [9.9.9.9,149.112.112.112]
```