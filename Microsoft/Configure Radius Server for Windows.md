# Configure Radius Server for Windows
#network #windows #powershell #sysadmin 

Import the configuration file if you have one:
```powershell
Import-NpsConfiguration -Path "C:\Npsconfig.xml"
```

Create a firewall rule to allow UDP port 1812 inbound for radius authentication.
```powershell
netsh advfirewall firewall add rule name=“Allow UDP 1812 Inbound Radius” dir=in action=allow protocol=UDP localport=1812 profile=Any
```

Install your radius certificate in the local computer personal store