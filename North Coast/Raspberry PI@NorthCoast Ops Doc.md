# Raspberry Pi IP’s
#northcoast #docs #iot #raspberrypi 

Chapel 1:			    172.20.12.41
Warehouse 2:		    172.20.13.10
Cafe 3:				172.20.13.99
EdgeLeft 4:			172.20.13.104
EdgeRight 5:			172.20.13.108
FamilyRoom 6:		    172.20.13.89
LiveLeft 7:			172.20.12.108
LiveRight 8:			172.20.12.127

## Connecting to Serial from within SSH
If screen is not installed you will need to install screen:

```
sudo apt install screen -y
```

## Connect to Serial

```
sudo screen /dev/ttyUSB0 57600
```
> To Detach from screen use ctrl+a and ctrl+d

Commands
```bash
sets the Wi-Fi network to NCC
set wlan.ssid “NCC”

sets the password for the Wi-Fi
set wlan.passkey “ShibbolethNCC”

  [should already be set] // sets monitors to retry 
set wlan.join.retries 3
I # connecting to the network at least 3 times if they lose connection(essential)

set MF.AutoRebootMins 60 // sets monitors to reboot every hour 
set MF.TCPpin 5378 //sets the monitors security pin to 5378
set MF.DeviceName *venue name* // sets the name of the baby monitor according to what venue it is in
set wlan.dhcp.cahce_enabled 0 // turns this setting off which fixes a problem of bad IP information causing a disconnect
get all //shows all the configuration settings
Save // saves all of your changes
Reboot // reboots the monitor 
Ver // gives you the current version of the firmware
Diag [our diagnostics] // shows our essential configuration settings
Nre [network restart] // restarts the network?? 
Ota [update firmware] // searches for the newest firmware
```