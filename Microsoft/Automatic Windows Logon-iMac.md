# Automatic Windows Logon
#windows #regedit #sysadmin 

Setup a user account for automatic logon.
Open Regedit and Locate the following subkey in the registry:

```powershell
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
```

Edit the following string values:

>if they dont exist you will need to create them.

```
edit the DefaultUserName entry, type your user name, and then click OK.
edit the DefaultPassword entry, type your password, and then click OK.
```

> Note If no DefaultPassword string is specified, Windows automatically changes the value of the AutoAdminLogon key from 1 (true) to 0 (false), disabling the AutoAdminLogon feature.
 
Create a new string value named: "AutoAdminLogon" and set this value to 1

> If you have joined the computer to a domain, you should add the DefaultDomain value, and the data for the value should be set as the fully qualified domain name (FQDN) of the domain.

Exit Registry Editor.

Restart your computer, it should logon automaticaly.