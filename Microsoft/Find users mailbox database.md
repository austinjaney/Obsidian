# Find users mailbox database
#sysadmin #exchange #powershell 

To what database a specific user is in run

```
Get-mailbox -identity "email address" | fl database
```

To list all users in a target database run

```
Get-Mailbox  -Database "Database Name"
```