# Reconnect an exchange mailbox
#sysadmin #exchange #powershell 

In Exchange PowerShell to enable or reconnect a recently disabled mailbox to its user run the following command.

```powershell
Connect-Mailbox -Identity "User Name" -Database EX13GeneralUser -User "User Name" -Alias uname
```
> The alias should match before the @domainname.com