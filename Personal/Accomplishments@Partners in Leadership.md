# Accomplishments@Partners in Leadership
#personal 

```
A list of completed projects at PIL
```

**Zerotier Implementation**

Implemented Zerotier with DNS that feeds back to our domain controllers allowing desktops and notebooks anywhere in the world to communicate with our servers and be managed as if they were on our LAN. This not only created a better user experience but also also allowed us to open up a work from home program so our employees can work from home. Some employees were even able to keep their jobs and move out of state.

**Jira Service Desk**

Implemented Jira Service desk with fine tuning allowing us to keep track of projects and have a unified platform for tracking and reporting on all IT issues, using jira reporting we are able to aggregate and parse issue data giving us insights into what's causing problems and how we can make better decisions. I also built a confluence wiki to create end user help-desk articles and keep track of our own internal documentation. Since our help-desk was completed I have started to expand and offer Jira to other teams as well enhancing productivity as a whole. 

**PDQ deploy (automated windows configuration)**

Implemented PDQ has given us the ability to remote manage workstations and automate large parts of user workstation deployment saving us time and enforcing standardization of applications on windows. Doing this allowed us to be ready to replace or re-image systems in minuets instead of days and migrate to windows 10 seamlessly. This also has given us the ability to install software remotely on systems without ever touching them.
> I have since improved on this further using powershell scripting by writing a tool that when used configures a windows image on a system in audit mode.  

**Implemented SPF, DMARC and DKIM**

I implemented SPF, DMARC and DKIM to better protect company email, this has also given us the ability to better sort spam and phishing attacks as well as protect our companies name from being used to conduct phishing attacks on our customers.

**DNS Filtering with OpenDNS**

As the primary resolver for our network which has given us enterprise grade filtering capabilities and has no doubt helped keep our in office systems much cleaner than they would have been otherwise.

**Registrar Migration to Hover**

I moved our domains to hover from network solutions. This will save us over $5000 annually in bogus registration fees and give us better privacy and functional features such as whois privacy on every domain and free domain forwarding services. In addition to that our domains are now much easier to manage.

**Lastpass Password Management**

Rolled out lastpass to the IT team so we have better security and coordination when managing credentials and software keys. This has kept us organized and safer allowing us to have stronger more complex passwords securing critical systems.

**Asset Management migration**

I migrated our Asset system to the cloud using asset tiger, we now can check assets in and out with our phones which makes us more prone to use the asset system and makes auditing take days instead of weeks. This system also allows us to easily import and export data via excel spread sheets and gives us more flexible reporting and the ability to use the data in other systems as needed.

**Migration to Backblaze for mac users**

When apple stopped supporting mac server as a time machine backup platform I migrated every in office Mac to Backblaze which now gives us cheap cloud based backups. This was done quickly using a scripted process which I hope will become the automatic standard when we eventually have JAMF.

**Automated desktop backup script**

I wrote a fully automated backup process for user desktops, we now have backups of all windows PC’s in 3 hour increments and on login using group policy and pdq deploy. These backups also work on remote workstations through Zerotier.

**Fortinet Overhaul and network rack management**

Our server room looks fantastic, so does our network, all of our network is now centrally managed I helped with this a lot and pulled long days and weekends to get our Fortinet network up and running. Before I started here there was a Fortinet firewall, that was configured to let any and all traffic into the network, by fixing this I potentially prevented [WannaCry](#WannaCry) from reaching our systems. (our domain controllers were on the internet with public IP’s).

**Automated user on-boarding process**

I wrote a powershell script that walks through our on-boarding process much the same way as adding a user to a linux system. It asks questions and generates tickets based on the answers as well as creating their active directory account and configuring their network home folder.

**Implemented script based auditing**

We now have Servers that send us status emails automatically every day and have the ability to run compliance types of scans on user machine to search for files that might be violation of company policy, such as password documents or customer data lists.

**Self Hosted cloud FTP**

I replaced our aging FTP server with a Nextcloud server hosted locally so our clients (even the ones that block cloud sharing services) would be able to receive files ensuring even when dropbox or hightail wouldn’t work we would have another option that would.

**iPass Deployment**

I deployed iPass to all of our field members which consolidated billing for internet access amongst our field sales team. I wrote a bash script to make deployment standardized which also made deployments quick and efficient.

**RingCentral Rooms**

I built and configured [RingCentral](#RingCentral) rooms to help make the training room and other facilities in our building easier to use and provide a better technology experience. This involved running new cabling to different locations and enabled us to host workshop format talks in house and enable better communication and employee training session. 

**Migration from Tape Backups**

I migrated us away from our old tape backup infrastructure to using Backblaze buckets for our server backups. Our new backup system backs files up in close to real time and does version history.

**Microsoft 365 [Work in Progress]**

We have used office 365 for quite some time at pil and now are looking to move all of our existing on prem infrastructure to the cloud, I have configured Microsoft 365 zero touch provisioning so that we can manage PC’s using windows in-tune mdm policies without ever touching the PC’s. 

JAMF [Work in Progress]