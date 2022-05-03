# Alpine PI
#raspberrypi #linux #alpine #microframes

Deployed Raspberry Pi's at Vista
Live Left  | ssh://root@172.20.12.108
Live Right | ssh://root@172.20.12.127
Edge Left  | ssh://root@172.20.13.104
Edge Right | ssh://root@172.20.13.108
Wearhouse  | ssh://root@172.20.13.10
FamilyRoom | ssh://pi@172.20.13.89
* Cafe       | ssh://root@172.20.13.99
* Chapel     | ssh://root@172.20.12.41
Austins-PI | ssh://root@172.20.12.7

> Austins-PI is the test PI, feel free to SSH in and try things just dont run the save config command.

## Common Raspberry PI Tasks in Alpine Linux

### Change the Hostname
```
echo "shortname" > /etc/hostname
hostname -F /etc/hostname
lbu commit -d
```

### Saving Configuration Changes

```
lbu commit -d
```
> by default all changes in alpine linux are cleared by a reboot just like a network switch, if you have made a change you will need to run this command to make it persist through reboots.

## Common Microframe Tasks

Connect to a serial device

```
screen /dev/ttyUSB0 57600
```

sets the Wi-Fi network to NCC

```
set wlan.ssid “NCC”
```

sets the password for the Wi-Fi

```
set wlan.passkey “ShibbolethNCC”
```

connecting to the network at least 3 times if they lose connection(essential)

```
set wlan.join.retries 3
```

sets monitors to reboot every hour 

```
set MF.AutoRebootMins 60
```

sets the monitors security pin to 5378

```
set MF.TCPpin 5378
```

sets the monitors security pin to 5378

```
set MF.TCPpin 5378
```

sets the name of the baby monitor according to what venue it is in

```
set MF.DeviceName *venue name*
```

turns this setting off which fixes a problem of bad IP information causing a disconnect

```
set wlan.dhcp.cahce_enabled 0
```

show all configuration settings

```
get all
```

saves all of your changes

```
save
```

reboots the monitor

```
Reboot
```

gives you the current version of the firmware

```
Ver
```

shows our essential configuration settings

```
diag
```

restarts the network?

```
Nre
```

searches for the newest firmware

```
Ota
```

## Alpine Linux Configuration Notes

### 1. Configure Wireless
```
wpa_passphrase 'NCC' 'TYPE-NCC-PASSWORD-HERE' > /etc/wpa_supplicant/wpa_supplicant.conf
```

Once that command has been run you will need to edit the config file: "/etc/wpa_supplicant/wpa_supplicant.conf" to include "scan_ssid=1" this will allow the PI to connect to hidden networks such as NCC, once done the config should look like the one below

```
network={
	ssid="NCC"
	scan_ssid=1
	#psk="ShibbolethNCC"
	psk=312e1d1b4dab822a7a9e01f36afd74f566784d30d4dc65f0b416e056c439553a
}
```

### 2. Test Wireless
Start wpa_supplicant in the foreground to make sure the connection succeeds.

```
wpa_supplicant -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```

### 3. Set Wireless configuration
If all is well, run it as a daemon in the background by setting the -B option.

```
wpa_supplicant -B -i wlan0 -c /etc/wpa_supplicant/wpa_supplicant.conf
```

Configure the interface with an IP address.

```
udhcpc -i wlan0
```

Sanity check: the interface should have an inet address.

```
ip addr show wlan0
```

Automatic Configuration on System Boot
Add a stanza for the desired interface (e.g. wlan0) to /etc/network/interfaces:

```
auto wlan0
iface wlan0 inet dhcp
```

Make sure /etc/wpa_supplicant/wpa_supplicant.conf is the correct configuration for the wireless access point you want to connect to.

Bring the interface down.

```
ifconfig wlan0 down
```

Manually start wpa_supplicant.

```
/etc/init.d/wpa_supplicant start
```

If all is well (confirm with the sanity checks in Manual Configuration), configure wpa_supplicant to start automatically on boot.

```
rc-update add wpa_supplicant boot
```

> Note: Dont forget to save your config to the PI with the command "lbu commit -d"