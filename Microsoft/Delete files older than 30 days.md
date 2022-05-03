# Delete files older than 30 days
#sysadmin #powershell

Delete Items Older than 30 days using Powershell.

```powershell
Get-ChildItem –Path "C:\path\to\folder" -Recurse | Where-Object {($_.LastWriteTime -lt (Get-Date).AddDays(-30))} | Remove-Item
```