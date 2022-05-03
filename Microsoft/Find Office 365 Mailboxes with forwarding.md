# Find Office 365 Mailboxes with Forwarding enabled
#powershell #office365 

```powershell
Get-Mailbox -RecipientTypeDetails SharedMailbox | Where {($_.ForwardingSMTPAddress -ne $null) -or ($_.ForwardingAddress -ne $null)} | Select Name, ForwardingSMTPAddress, ForwardingAddress, DeliverToMailboxAndForward | export-csv c:\temp\forwards.csv 
```