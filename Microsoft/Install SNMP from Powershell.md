# Install SNMP from Powershell
#sysadmin #windows #server 

Install SNMP Feature
```powershell
Install-WindowsFeature SNMP-Service
```

It looks like sometimes windows server does not add all the required SNMP RSAT tools which means some tabs don’t appear in services.msc for the SNMP service to add those open command prompt as an admin and run

```powershell
Dism /online /enable-feature /featurename:Server-RSAT-SNMP /ALL
```