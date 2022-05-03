# Clear Previous or Duplicate Mailbox Info
#office365 #sysadmin #exchange #powershell 

Sometimes a user with an on premise Exchange Mailbox user will already have an exchange mailbox in Exchange Online. This can create an issue when you go to migrate the user from Exchange to the cloud since you have a duplicate mailbox.

To check this you can connect to your on premise exchange and connect to exchange online to verify that the user has more than one mailbox, once verified connect to exchange online and run the following command:

```powershell
Connect-EXOPSSession
Set-User Jon@contoso.com -PermanentlyClearPreviousMailboxInfo
```

Source: https://techcommunity.microsoft.com/t5/exchange-team-blog/permanently-clear-previous-mailbox-info/ba-p/607619

Source: https://www.multicastbits.com/hybrid-exchange-mailbox-on-boarding-target-user-already-has-a-primary-mailbox-fix/