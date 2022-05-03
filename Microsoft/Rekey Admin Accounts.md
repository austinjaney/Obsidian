# Rekey Admin Accounts
#sysadmin #powershell #windows 

Create new admin account and remove old admin account on windows

```powershell
Net User <newusername> <password> /ADD
Net LocalGroup Administrators <newusername> /ADD
Net User <previoususername> /DELETE
```