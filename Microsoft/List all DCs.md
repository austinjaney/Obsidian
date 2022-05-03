# Get a list of all domain controllers
#sysadmin #activedirectory #powershell 

```powershell
Get-ADDomainController -Filter * | select name, operatingsystem
```