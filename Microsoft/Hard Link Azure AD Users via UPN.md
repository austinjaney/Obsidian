# Hard Link Azure AD Users via UPN
#sysadmin #azure #powershell #office365 

Way 1 by using AD username and office 365 email address

```powershell
Connect-MsolService
$ADUser = "ajaney" 
$365User = "ajaney@northcoastchurch.com"
$guid =(Get-ADUser $ADUser).Objectguid
$immutableID=[system.convert]::ToBase64String($guid.tobytearray())
Set-MsolUser -UserPrincipalName "$365User" -ImmutableId $immutableID
```

Way 2 by using the common name or "Full name" which should match between both systems.

```powershell
#CommonName
$cn = “Erika Hester”

#Get the AD User ObjectGUID
$guid = (get-aduser -f {cn -eq $cn} -pr objectguid).objectguid

#Get the AD User UPN (matching the Azure AD User Object UPN)
$upn = (get-aduser -f {cn -eq $cn}).userprincipalname

#Convert the ObjectGUID into a ImmuteableID
$ImmutableID = [System.Convert]::ToBase64String($guid.ToByteArray())

#Set the ImmuteableID to the Azure AD User Object
set-msolUser -userprincipalname $upn -immutableID $ImmutableID
```

### See users with current sync errors
```powershell
Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict
```