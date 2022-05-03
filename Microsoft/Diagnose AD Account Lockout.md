# Diagnose AD Account Lockout
#sysadmin #activedirectory #support

Print lockouts in Powershell

```
$properties = @(
    @{n='User';e={$_.Properties[0].Value}},
    @{n='Locked by';e={$_.Properties[1].Value}},
    @{n='TimeStamp';e={$_.TimeCreated}}
    @{n='DCname';e={$_.Properties[4].Value}}
)
Get-WinEvent -ComputerName DCV1A -FilterHashTable @{LogName='Security'; ID=4740} | 
Select $properties | ft
```

Export a CSV of lockouts

```
$properties = @(
    @{n='User';e={$_.Properties[0].Value}},
    @{n='Locked by';e={$_.Properties[1].Value}},
    @{n='TimeStamp';e={$_.TimeCreated}}
    @{n='DCname';e={$_.Properties[4].Value}}
)
Get-WinEvent -ComputerName DCV1A -FilterHashTable @{LogName='Security'; ID=4740} | 
Select $properties |
Export-csv C:\Users\$env:username\Desktop\LockedUsers.csv -NoTypeInformation -Encoding UTF8
```