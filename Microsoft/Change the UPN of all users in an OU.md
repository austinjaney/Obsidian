# Change the UPN of all users in an OU
#office365 #activedirectory #powershell 

Running this will change the UPN of all users within a given OU.

```powershell
Import-Module ActiveDirectory
$oldSuffix = "ncc.lan"
$newSuffix = "northcoastchurch.com"
$ou = "OU=Preschool,OU=Staff,OU=Accounts,OU=NORTH COAST,DC=ncc,DC=lan"
$server = "DCV2A"
Get-ADUser -SearchBase $ou -filter * | ForEach-Object {
$newUpn = $_.UserPrincipalName.Replace($oldSuffix,$newSuffix)
$_ | Set-ADUser -server $server -UserPrincipalName $newUpn
}
```