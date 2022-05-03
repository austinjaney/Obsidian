# Install Zulip in an LXD Container
#linux #ubuntu 

Install LXD and add add-it to the lxd group, log out and log back in.

```bash
sudo adduser adm-it lxd
```

### Setup Networking
Changing nictype to macvlan, lxc profile edit default will open the profile with nano.

```bash
lxc profile edit default
```

### Change
```bash
nictype: bridged To -> nictype: macvlan
parent: whatever To -> parent: your primary nic name (enp0s25 in my case)
```

### Build Zulip container
```bash
lxc launch ubuntu:18.04 zulip
```

### Working with YAML for Netplan in containers
```bash
Sudo nano /etc/netplan/50-cloud-init.yaml
``````bash
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

### Download Zulip
```bash
cd $(mktemp -d)
wget https://www.zulip.org/dist/releases/zulip-server-latest.tar.gz
tar -xf zulip-server-latest.tar.gz
```

### Update Ubuntu
```bash
sudo apt update && sudo apt upgrade -y
```

### Install Zulip
```bash
sudo -s ./zulip-server-*/scripts/setup/install —certbot \—email=ajaney@northcoastchurch.com —hostname=zulip.northcoastchurch.com
```