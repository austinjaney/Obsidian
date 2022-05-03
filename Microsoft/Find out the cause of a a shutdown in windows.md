# Find who shutdown windows with powershell
#powershell #windows 

Run this command on the server in question to find out who shut down the system:

```powershell
$properties = @(
    @{n='TimeStamp';e={$_.TimeCreated}},
    @{n='Who did it';e={$_.Properties[6].Value}},
    @{n='Reason';e={$_.Properties[0].Value}},
    @{n='Action';e={$_.Properties[4].Value}}
)
Get-WinEvent -FilterHashTable @{LogName='System'; ID=1074} | 
Select $properties | Sort-Object "$_.TimeCreated"
```