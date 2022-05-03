# Delete Azure AD Users
#sysadmin #azure #office365 #powershell

> if you have not connected before you need to run:
```powershell
Install-Module MSOnline
Install-Module AzureAD
```

## Command to connect to microsoft online using modern auth

```powershell
Connect-MsolService
```

### Remove a user from office 365
```powershell
Remove-MsolUser -UserPrincipalName ‘DemoUser5@Priasoft.mail.onmicrosoft.com’
```

### Perminatly and Immediatly delete a user
```powershell
Remove-MsolUser -UserPrincipalName ‘DemoUser5@Priasoft.mail.onmicrosoft.com’ -RemoveFromRecycleBin
```
### Empty the Active Directory recycle bin
```
Get-MsolUser -ReturnDeletedUsers | Remove-MsolUser -RemoveFromRecycleBin –Force
```

> Doing this will remove a an azure AD only user, to remove local users they must be removed from local active directory first.
