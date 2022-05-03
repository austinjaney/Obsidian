# Daily Note March 17 2020
#day 

Office 365 OWA links to on prem webmail.

Links from microsoft support
https://social.technet.microsoft.com/wiki/contents/articles/921.simplify-the-outlook-web-app-url-in-exchange-server-2010.aspx

https://docs.microsoft.com/en-us/exchange/simplify-the-outlook-web-app-url-exchange-2013-help

https://docs.microsoft.com/en-us/exchange/hybrid-deployment/simplify-owa-url


Configuring Office 365 OWA
https://docs.microsoft.com/en-us/exchange/hybrid-deployment/simplify-owa-url

https://social.technet.microsoft.com/wiki/contents/articles/921.simplify-the-outlook-web-app-url-in-exchange-server-2010.aspx

Get-OrganizationRelationship | FL

```
[PS] C:\Windows\system32>Get-OrganizationRelationship | FL


RunspaceId            : ecd785c0-b409-4725-bdc7-f3862165264d
DomainNames           : {northcoastchurch.mail.onmicrosoft.com}
FreeBusyAccessEnabled : True
FreeBusyAccessLevel   : LimitedDetails
FreeBusyAccessScope   :
MailboxMoveEnabled    : True
DeliveryReportEnabled : True
MailTipsAccessEnabled : True
MailTipsAccessLevel   : All
MailTipsAccessScope   :
PhotosEnabled         : True
TargetApplicationUri  : outlook.com
TargetSharingEpr      :
TargetOwaURL          : http://outlook.com/owa/northcoastchurch.onmicrosoft.com
TargetAutodiscoverEpr : https://autodiscover-s.outlook.com/autodiscover/autodiscover.svc/WSSecurity
OrganizationContact   :
Enabled               : True
ArchiveAccessEnabled  : True
AdminDisplayName      :
ExchangeVersion       : 0.10 (14.0.100.0)
Name                  : On-premises to O365 - 0fbd5031-9d75-482c-90d9-4d6501e88168
DistinguishedName     : CN=On-premises to O365 -
                        0fbd5031-9d75-482c-90d9-4d6501e88168,CN=Federation,CN=NorthCoastChurch,CN=Microsoft
                        Exchange,CN=Services,CN=Configuration,DC=ncc,DC=lan
Identity              : On-premises to O365 - 0fbd5031-9d75-482c-90d9-4d6501e88168
Guid                  : 8d9371f0-ace9-46f8-8861-4df35c561011
ObjectCategory        : ncc.lan/Configuration/Schema/ms-Exch-Fed-Sharing-Relationship
ObjectClass           : {top, msExchFedSharingRelationship}
WhenChanged           : 3/16/2020 4:27:59 PM
WhenCreated           : 3/16/2020 4:27:57 PM
WhenChangedUTC        : 3/16/2020 11:27:59 PM
WhenCreatedUTC        : 3/16/2020 11:27:57 PM
OrganizationId        :
Id                    : On-premises to O365 - 0fbd5031-9d75-482c-90d9-4d6501e88168
OriginatingServer     : DCV1a.ncc.lan
IsValid               : True
ObjectState           : Unchanged
```