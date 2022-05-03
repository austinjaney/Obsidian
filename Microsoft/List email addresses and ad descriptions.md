# Get a list of user emails and descriptions
#sysadmin #powershell #exchange 

Powershell Query to get a list of user emails and descriptions

Run in exchange but import the AD module first.

```powershell
Get-Mailbox *@northcoastchurch.com | Select-Object -ExpandProperty Alias | Get-ADUser -filter * -Properties EmailAddress, description | ft EmailAddress, description > C:\emailsanddescriptions.txt
```