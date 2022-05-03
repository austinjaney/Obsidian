# Office 365 Whitelisting
#sysadmin #office365 

Some services like docusign, dropbox, salesforce, and others send users messages for things like password resets, sometimes these messages end up in spam.  Upon first glance you might think ok well I will just whitelist the the domain and then that wont happen but with the rise of targeted phishing attacks we have a new problem.

Whitelisting the domain docusign.com will also allow people who spoof docusign.com to have their mail delivered to user inboxes.  To get around this I create a rule in exchange.

The rule reads something like this:

```
If the message...
'Received-SPF' header matches the following patterns: 'lastpass.com' or 'roberthalf.com' or 'ringcentral.com' or 'letsencrypt.org' or 'taxjar.com' or 'login.gov' or 'docusign.net' or 'cvent-planner.com' or 'sendgrid.piltools.com' or 'atlassian.net' or 'salesforce.com'or...
Do the following...

Set the spam confidence level (SCL) to '-1'
and set message header 'X-MS-Exchange-Organization-BypassClutter' with the value 'true'
and Stop processing more rules
and Send the incident report to exchangediagnostics@partnersinleadership.com, include these message properties in the report: sender, recipients, subject, cc'd recipients, bcc'd recipients, severity, sender override information, matching rules, false positive reports, detected data classifications, matching content, original mail
```

doing this allows mail from some senders to always pass through, this is critical for smooth operation and clean comunication between different corperate applications.

Depending on your antispam setting it may also be helpful to take a look and see how your exchange online service is handling mail that does not match SPF and DKIM and if your mail servers are using DMARC to inturperate incoming mail. 