# Force AD Replication on all sites at once
#powershell #activedirectory #sysadmin

Run the following in Powershell ISE on a domain controller as a domain admin:

```powershell
function Replicate-AllDomainController {
(Get-ADDomainController -Filter *).Name | Foreach-Object {repadmin /syncall $_ (Get-ADDomain).DistinguishedName /e /A | Out-Null}; Start-Sleep 10; Get-ADReplicationPartnerMetadata -Target "$env:userdnsdomain" -Scope Domain | Select-Object Server, LastReplicationSuccess
}
```

Then run:

```powershell
Replicate-AllDomainController
```

From Microsoft Support

```powershell
repadmin /syncall /ADeP
```