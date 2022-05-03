# Proofpoint@northcoast
#northcoast #exchange #docs 

### Whitelisting
Proofpoint functions in a similar way to SPF, we will need to add senders that we know can spoof our domain.  To look for messages that do not meet the spoof policy look under:
System -> Quarantine -> Folders -> and Spoofed.

To whitelist a sender that might be spoofing a change to the policy route will be needed.
System -> Policy Routes -> pp_spoofsafe -> add condition

To whitelist a Sender that’s being marked as Bulk we have created a custom Spam Detection rule
Email Protection -> Spam Detection -> Custom Rules -> bulk_mail_bypass

### adding a protected user
Protected users are guarded from phishing and impersonation attempts.
Email Protection -> Spam Detection > Settings > Impostor Display Names

### DMARC

### Support Contact info
proofpointcommunities.force.com

```
Aymen Saad, C|EH
Sales Engineer
Office: +1-408-850-4186
Email: asaad@proofpoint.com
``````
Michael Chapman
Account Manager
Office: (408)585-4334
Email: mchapman@proofpoint.com
```

To-do
- logs and reports, report publisher, saved, go and create some reports!
- [Configure recipiant verification / user import. Azure AD Sync Required i think.](agenda://note/42EDCB22-2DC7-48C1-B537-0BDDA4A52F90)
+ Configure DKIM Keys￼
+ look at forcepoints DMARC settings and see if they are strict￼
+ Configure spam detection, settings, Imposter display names.  We need to add our SLT and permitted external email addresses.
+ Write an email to jluccion@proofpoint.com with the message we want users to see notifing them that we were unable to do an antivirus scan on a message and have quarintineed it.￼
“You have recieved a message that could not be scanned for malware, we believe that it might be risky if this looks like an importaint message please reach out to computerhelpdesk@northcoastchurch.com and we will release the message to you.”

## email from Joel:
```
“Sounds good, also below is the updated DKIM record to be uploaded where you manage DNS for your domain:
Selector:
northcoastchurch09162019._http://domainkey.northcoastchurch.com. 
DKIM Record:
v=DKIM1; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAt0Sr+bAnzRQOknluVSYIaHQhWeiE3vt4H9O3SNI4X0P/dhYBoAUclOzA/mDS4/C09qullatcF31TjeW+XFXWhna3V6jrIuaBRUgdPRwUTvUTE63AXuJtc5Aw8FbOiI09rePXifaQN06CdTd64N+UzjNsepgk+wJikpNb3ua5tupgdadr2luNonkhuRWVPR5h70YHcB2LZy1GQJ6eHtq8nPkmv2hJyNkrr89J/RocbEM16BK2GxMfcAnOdcHKi4p4OhU4GKi7D3pU0qBWOCCnOFkPpf8Dakq83I3zmxT2uZBrmQD+IYD2MFZ9SmNbrKDzj05saM4uItoeUDh0Ba+UvQIDAQAB


Joel Luccion
Professional Services Consultant 
Proofpoint, Inc.
E: [jluccion@proofpoint.com”](mailto:jluccion@proofpoint.com)
```

```
Proofpoint on Demand - Enterprise
#################################
Welcome North Coast Church!

Thank you for using Proofpoint on Demand Enterprise.

Based upon the information you have provided, we have created a profile for your
organization and are ready to start filtering and managing your email.

The following provides information on how to access your service.


Proofpoint Administrative GUI

address:  https://004ad901.pphosted.com:10000

These passwords should be changed on your first login.


Proofpoint on Demand Enterprise - End User GUI (if enabled)

https://004ad901.pphosted.com:10020

A random default password has been set for end-user access. You may reset the
default password in Groups and Users/Profiles/<PPS or other profile>/Advanced,
or you may set up PPS to use an external authorization source.

NOTE: The administration and end-user web applications are reached via the ports
above. If your company blocks non-standard ports at the firewall, you will need
to create an exception for these ports.


MX Records
==========

Please change your public DNS entries to the following records (note the
trailing "."!):

<yourdomain.tld>. 1800 IN MX 10 http://mxa-004ad901.gslb.pphosted.com.

<yourdomain.tld>. 1800 IN MX 10 http://mxb-004ad901.gslb.pphosted.com.

Repeat for each domain you route through Proofpoint on Demand. Note that the
Proofpoint on Demand service must be configured for any additional domains
prior to routing mail through it. All domains listed on your Pre-Deployment
Questionnaire have already been configured.


Firewall Settings - Inbound
===========================

Please allow port 25 (SMTP) access to your mail servers(s) from the following
hostnames/IPs.  These are your dedicated "Virtual IP" (VIP) addresses:

http://mx0b-004ad901.pphosted.com. 205.220.174.20
http://mx0a-004ad901.pphosted.com. 205.220.162.21

NOTE: If you do not restrict your firewall to allow only incoming mail from the
Proofpoint on Demand service, you will likely receive mail sent directly to your
mail server which will not be filtered by Proofpoint on Demand. This may cause
your end users to believe that spam is getting through Proofpoint filtering,
when in reality the messages are not being scanned at all.

It is recommended that after you configure your firewall, you contact your
Proofpoint Sales Engineer to assist with testing mail flow from Proofpoint on Demand
prior to changing your MX records.

If you have requested that your service be configured for regular LDAP imports,
you will also need to provide either port 389 (LDAP) or port 636 (LDAPS) access
to your LDAP or AD server from your Proofpoint Virtual IPs.


Outbound Mail Configuration
===========================

If you are sending mail outbound through your Enterprise deployment, please send
mail to the following hosts:

http://mxa-004ad901.gslb.pphosted.com

http://mxb-004ad901.gslb.pphosted.com

For redundancy, you must list all hosts by hostname in your outbound mail
configuration, in a round-robin or ordered fashion.  Please do not hard-code the IP
addresses for these hosts, as it will impact our ability to provide you with
the best redundancy possible.


Sender Policy Framework (SPF) Records
=====================================

If you are sending outbound mail through Proofpoint on Demand, it is very
important that you modify your domain's DNS TXT records to include an "SPF"
record for your domain. An SPF record is a way for a receiving mail system to
determine if a sending server should be considered valid for the address listed
in the "From" header. Without these records, some recipients (including AOL) may
rate control or otherwise limit connections from Proofpoint's servers because
the servers are part of Proofpoint's network, rather than your organization's.

A simple way to make this change is to add the following as a DNS TXT record to
your domain:

"v=spf1 include:http://spf-004ad901.pphosted.com ~all"

You can, of course, add any additional values in your SPF records as necessary.

By using the "include" syntax, we will be able to dynamically serve the
addresses used for your cluster and keep it up to date for you with no
maintenance required on your part.

For more information on SPF records, you can refer to the OpenSPF site:
http://www.openspf.net/

The "include" syntax is specifically described here:
http://www.openspf.net/SPF_Record_Syntax#include


Further Information
===================

Please login to the Proofpoint Customer Success Center to access key resources:

–	Customer Support
–	Product Discussion Forums
–	Knowledge Base Access
–	Admin Guides and Release Notes
–	Online Case Tracking and Updates
–	News Channels

https://proofpointcommunities.force.com/community/

Organized by Proofpoint Professional Services, with Marty Hudacko, Michael Ghany, Joel Luccion, and Alan Haskel, at Remote - 408-638-0968,, 2809368870#.
Notes from Calendar Event:
Hello Austin,

Per your email, I have scheduled a project working session with Proofpoint for Friday September 13th 10am PDT. Your Professional Services Consultant, Joel Luccion, will be leading this call with you.

Phone Conference Information:
+1-408-638-0968
Meeting ID: 280-936-8870
https://proofpoint.zoom.us/j/2809368870

International numbers available:
https://proofpoint.zoom.us/zoomconference?m=HQTLuYpHXvynCrzbrNNazfYU-AG2Zjfw

Here are contact details for your reference:

Proofpoint:
Joel Luccion
Professional Services Consultant
jluccion@proofpoint.com<jluccion@proofpoint.com>
(801) 508-4636

North Coast Church:
Austin Janey
ajaney@northcoastchurch.com<ajaney@northcoastchurch.com>
Time Zone: Pacific

We look forward to working with you.  Please let me know if you have any questions, or if there is any change to your schedule.

Thanks and have a great day,
Shorwei


Shorwei Gong
Project Coordinator, Professional Services
Proofpoint, Inc.
408-850-4124
sgong@proofpoint.com
```