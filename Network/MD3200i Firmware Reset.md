# MD3200i Firmware Reset
#sysadmin #dell #hardware 

#### How to connect to the console on a Dell MD storage array
To enter the console for an MD series storage controller you will need the service cable (PN: MN657).
Once connected to the controller and a serial port, use putty to login to the controller.

#### Connection settings
* baud:		115200
* data bits:	8
* stop bits:	1
* parity:		None

To enter the service menu press Ctrl-Break and then press S (case sensitivity).
The default password is *supportDell*

Here you can view and change the management IP address.
If you are having serious issues with the array and need to get to lower level commands, you can do the following.

Press Ctrl-Break Instead of hitting S press Esc You should get a vxWorks login:
Username:	shellUsr
password:	DF4m/2>

Note: it’s much easier to copy and paste the user/password into the putty terminal. Just right click to paste.

Common commands:

```
lemClearLockdown
sysWipeAllConfigData
```

> Note: the above command wipes the configuration. If you have data you need, do NOT run the above command. You can type /SysWipe/ to get a list of all reset commands, including nondestructive options.  

```
netCfgShow
```