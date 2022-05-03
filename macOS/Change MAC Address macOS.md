# Changing your MAC Address in macOS
#macos #apple #sysadmin

You can change the MAC address yourself. I think it reverts upon reboot though.

The command is 

```bash
sudo ifconfig <interface> ether <new mac address>
```


For example, 

```bash
sudo ifconfig en0 ether 12:34:56:12:34:56
```


You may need to toggle the interface as well

```bash
sudo ifconfig en0 down
sudo ifconfig en0 up
```


As per this site (https://blog.macsales.com/37428-tech-tip-finding-and-changing-your-mac-address-in-os-x) you can make a random MAC like this


```bash
openssl rand -hex 6 | sed 's/\(..\)/\1:/g; s/.$//'
```
