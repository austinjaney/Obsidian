# Exchange Powershell Calendars
#office365 #exchange #sysadmin #powershell

List Calendar Permissions
This example will list the current permissions for Taman’s mailbox:

```powershell
Get-MailboxFolderPermission -Identity <taman.dawson@partnersinleadership.com>:\\calendar
```

Results should look something like:

```powershell
PS C:\\Users\\austin.janey\\Desktop\> Get-MailboxFolderPermission -Identity taman.dawson@partnersinleadership.com:\\calendar
```

```powershell
PS C:\Users\austin.janey\Desktop> Get-MailboxFolderPermission -Identity taman.dawson@partnersinleadership.com:\calendar

FolderName           User                 AccessRights                       SharingPermissionFlags
----------           ----                 ------------                       ----------------------
Calendar             Default              {AvailabilityOnly}
Calendar             Anonymous            {None}
Calendar             PIL Accts - Dont ... {Reviewer}
```

* The name of the user with permissions can be displayed under the User column of this output and can be used surrounded by quotes in at the end of the remove folder permissions command to specify who’s permissions are being removed.

#### Remove Folder Permissions
This example removes permissions on a per account access level. 

Supporting Microsoft documentation <https://docs.microsoft.com/en-us/powershell/module/exchange/mailboxes/remove-mailboxfolderpermission?view=exchange-ps>

```powershell
Remove-MailboxFolderPermission -Identity facilitatormailbox@partnersinleadership.com:\calendar -User “permission holder"
```

#### Setting Permissions and Adding Permissions
This example sends the sharing invitation from Jon to Taman and grants Taman editor rights to Jon’s calendar.

```powershell
Set-MailboxFolderPermission -Identity jon.hamilton@partnersinleadership.com:\Calendar -User Taman.dawson@partnersinleadership.com -AccessRights Editor -SharingPermissionFlags Delegate -SendNotificationToUser $true
```

#### List Calendar Permissions
```powershell
Get-MailboxFolderPermission -Identity taman.dawson@partnersinleadership.com:\\calendar | Format-List
```

#### Remove Folder Permissions
This example removes John's permissions to the Training folder in Kim's mailbox.

```powershell
Remove-MailboxFolderPermission -Identity kim@contoso.com:\\Training -User john@contoso.com
```