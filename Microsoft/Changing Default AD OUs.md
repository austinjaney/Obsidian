# Change the default OUs for Users and Computers
#activedirectory #windows #sysadmin 

Set the default OU for new computers to land in using redircmp

```powershell
C:\Windows\system32>redircmp "OU=New Computers,OU=Computers,OU=NORTH COAST,DC=ncc,DC=lan"
Redirection was successful.
```

Set the default OU for new user accounts to land in using redirusr

```powershell
C:\Windows\system32>redirusr "OU=New Accounts,OU=Accounts,OU=NORTH COAST,DC=ncc,DC=lan"
Redirection was successful.
```