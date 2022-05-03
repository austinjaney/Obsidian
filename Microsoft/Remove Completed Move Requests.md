# Remove Completed Move Requests
#sysadmin #powershell #exchange 

Clear all completed move requests from powershell

```powershell
Get-MoveRequest -movestatus completed | remove-moverequest
```

Get all completed move requests:

```powershell
Get-MoveRequest -resultsize unlimited | Where-Object {$_.status -like "Completed"} | Get-MoveRequestStatistics | select DisplayName, StatusDetail, *Size, *Percent* | ft
```

You can also do a small modification here to get only the non completed one via:

```powershell
Get-MoveRequest -resultsize unlimited | Where-Object {$_.status -notlike “Completed”} | Get-MoveRequestStatistics | select DisplayName, StatusDetail, *Size, *Percent* | ft
```

If you wish to get the failed one then you can use:

```powershell
Get-MoveRequest -resultsize unlimited | Where-Object {$_.status -like “failed”} | Get-MoveRequestStatistics -IncludeReport | select DisplayName, Message, FailureType, FailureSide, FailureTimeStamp, *bad*, *large*, Report, Identity | fl
```