# Find mailbox sizes
#sysadmin #powershell #exchange

Get a list of mailboxes and sizes via exchange powershell and export them to a file:

```
Get-Mailbox -ResultSize Unlimited | Get-MailboxStatistics | Sort-Object TotalItemSize -Descending | Select-Object DisplayName,TotalItemSize -First 100 | Export-CSV top100mailboxes.csv
```

Print the top 100 mailboxes in powershell

```
Get-Mailbox -ResultSize Unlimited | Get-MailboxStatistics | Sort-Object TotalItemSize -Descending | Select-Object DisplayName,TotalItemSize -First 100
```
