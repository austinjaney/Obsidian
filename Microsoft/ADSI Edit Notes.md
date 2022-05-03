# ADSI Edit Notes
#sysadmin #exchange #notes 

A list of shortcuts for ADSI Edit information

## Exchange ServiceBindingInformation
Changing this can help with issues related to having the wrong certificate notices in outlook and other applications.

Configuration -> CN=Configuration,DC=ncc,DC=lan -> CN=Services -> CN=Microsoft Exchange -> CN=NorthCoastChurch -> CN=Administrative Groups -> CN=Servers -> CN=EXMBXV3 -> CN=Protocols -> CN=Autodiscover -> CN=EXMBXV3
> Right Click and go to properties and then edit the URL for  “ServiceBindingInformation”