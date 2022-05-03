# Office 365 Best Pratices
#office365 #powershell #bestpratice #sysadmin 

Setup Notes and tweaks:

## General Best Pratices
Self Service Purchasing has been disabled across the board:

```powershell
Install-Module -Name MSCommerce #once you install you should remove this line
Import-Module -Name MSCommerce 
Connect-MSCommerce #sign-in with your global or billing administrator account when prompted
Get-MSCommerceProductPolicies -PolicyId AllowSelfServicePurchase | forEach { 
Update-MSCommerceProductPolicy -PolicyId AllowSelfServicePurchase -ProductId $_.ProductID -Enabled $false  }
```

Group Creation has been limmited so that only users of the group "Group Admins" security group are allowed to create office 365 groups:
https://docs.microsoft.com/en-us/office365/admin/create-groups/manage-creation-of-groups?view=o365-worldwide

# Office 365 Best Pratices From Sysadmin Today

1. Establish main tenant administrator with strong password and MFA.
2. Enable/Verify that modern authentication is enabled.
3. Setup tenant profile with organization information: [https://admin.microsoft.com/AdminPortal/Home\#/companyprofile](https://admin.microsoft.com/AdminPortal/Home#/companyprofile)
4. Configure Account Recovery Options: <https://aka.ms/ssprsetup>
5. Grant Delegated Admin to CSP (if MS Partner)
6. Configure Tenant alerts email addresses

```powershell
Set-AzureADTenantDetail -SecurityComplianceNotificationMails "alerts@domain.com" - TechnicalNotificationMails "alerts@domain.com" -MarketingNotificationEmails "alerts@domain.com"
```

Enable Unified Audit Logging 

```powershell
Set-AdminAuditLogConfig -UnifiedAuditLogIngestionEnabled $True
```

Enable Mailbox Audit Logging 

```powershell
$AuditSettings = @{
AuditEnabled = $True AuditLogAgeLimit = 365 AuditOwner =
"Create,HardDelete,MailboxLogin,Move,MoveToDeletedItems,SoftDelete,Update,UpdateCale ndarDelegation,UpdateFolderPermissions,UpdateInboxRules"
AuditDelegate = "Create,FolderBind,HardDelete,Move,MoveToDeletedItems,SendAs,SendOnBehalf,SoftDelet e,Update,UpdateFolderPermissions"
AuditAdmin = "Copy,Create,FolderBind,HardDelete,Move,MoveToDeletedItems,SendAs,SendOnBehalf,Soft Delete,Update,UpdateCalendarDelegation,UpdateFolderPermissions,UpdateInboxRules"
}
```

```powershell
Get-Mailbox | Set-Mailbox @AuditSettings
```

Set Language and Time Zone for All Users 

```powershell
Get-Mailbox | Get-MailboxRegionalConfiguration | ? {$_.TimeZone -eq $null} | Set-MailboxRegionalConfiguration -Language 1033 -TimeZone "Central Standard Time"
```

Increase Deleted Item Retention from 14 to 30 Days 

```powershell
Get-Mailbox -ResultSize unlimited | Set-Mailbox -RetainDeletedItemsFor 30
```

Show [MailTip](evernote-html-snippet://#MailTip) for External Recipients 

```powershell
Set-OrganizationConfig -MailTipsExternalRecipientsTipsEnabled $True
```

Show [MailTip](evernote-html-snippet://#MailTip) for Large \# of Recipients (Shows tip Beyond threshold) 

```powershell
Set-OrganizationConfig -MailTipsLargeAudienceThreshold 10
```

Outbound Spam Notifications 

```powershell
Set-HostedOutboundSpamFilterPolicy Default -NotifyOutboundSpam $true - NotifyOutboundSpamRecipients “alerts@domain.com”
```

Prevent Inbox Rules Forwarding Messages Externally 

```powershell
Set-RemoteDomain Default -AutoForwardEnabled $false
```

Prepend Disclaimer on External Messages 

```powershell
$TransportSettings = @{
Name = 'External Sender Warning'
FromScope = 'NotInOrganization'
SentToScope = 'InOrganization'
ApplyHtmlDisclaimerLocation = 'Prepend' ApplyHtmlDisclaimerText = "<p><div style='border:solid #9C6500
1.0pt;padding:2.0pt 2.0pt 2.0pt 2.0pt'><p class=MsoNormal style='line- height:12.0pt;background:#FFEB9C'><b><span style='font- size:10.0pt;color:#9C6500'></span></b><span style='font- size:10.0pt;color:black'>[EXTERNAL]<o:p></o:p></span></p>"
ApplyHtmlDisclaimerFallbackAction = 'Wrap' }
```

```powershell
New-TransportRule @TransportSettings
```

Increase [OneDrive](evernote-html-snippet://#OneDrive) Deleted User Retention (up to 3650 days) I think its 365 

```powershell
Set-SPOTenant -OrphanedPersonalSitesRetentionPeriod 180
```

### Disable IMAP / POP Protocols 
Current Users 

```powershell
Get-CASMailbox -Filter {ImapEnabled -eq "true" -or PopEnabled -eq "true" } | Select-Object @{n = "Identity"; e = {$_.primarysmtpaddress}} | Set-CASMailbox -ImapEnabled $false -PopEnabled $false
```

Future Users 

```powershell
Get-CASMailboxPlan -Filter {ImapEnabled -eq "true" -or PopEnabled -eq "true" } | set-CASMailboxPlan -ImapEnabled $false -PopEnabled $false
```


Have an Office 365 Backup Policy in place.
Enable DKIM and DMARC
Setup Anti-SPAM and Anti-Phishing policies
Use email encryption
Setup Alert Polices. (Creation of forwarding/redirect rule, permissions changes, Unusual volume of external file sharing etc)
Use MDM for all mobile devices that store company data.
Use Rights Management.
Setup compliance reviewers for eDiscovery and Supervision.
DLP Policies: * Setup External Sharing Polices * Define what data is important (SSN, CC, Health Records, Salaries, Accounts, PST MSG etc) * Create notifications when users share external data based on your defined criteria. * Take it to the next level in your policy and prevent data from being sent that matches the criteria.
Secure  [SharePoint](evernote-html-snippet://#SharePoint)  with multiple doc libraries.
Always use group level permissions. Use custom permission levels. (i.e. edit but not delete)
Disable anonymous guest link sharing

### Azure AD Notes (Some features may require advanced licensing like Azure AD Premium P1)
* Enable Group-Based Licensing to automatically assign O365 Licenses based on AD group
* Branding: Customize logos and colors to provide a branded and recognizable logon
* experience. Train users to not enter their password if they don’t see the branding
* MFA: Enable MFA for all users
* Trusted IPs: Configure trusted IPs to bypass MFA for the office; This will ease resistance to the MFA burden for end users
* Review Secure Score and work to increase it:  [https://securescore.office.com/](https://securescore.office.com/) 
* Use Conditional Access to prevent all access from outside your country (be sure to exclude
* an account as a safety measure against accidentally locking yourself out of the whole tenant)
Reporting Audits ( [PowerShell](evernote-html-snippet://#PowerShell) )
*  [LicenseReconciliationNeeded](evernote-html-snippet://#LicenseReconciliationNeeded)  (Mailboxes at risk of being deleted due to lack of license)
* Tenants with more licenses than consumed (wasting money)
* Users without MFA Enabled/Enforce