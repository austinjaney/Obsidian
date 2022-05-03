# DHCP Migration
#northcoast #windows #notes 

Read first
https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831447(v%3dws.11)

https://searchwindowsserver.techtarget.com/tip/Moving-DHCP-database-to-a-new-server

https://www.systemcodegeeks.com/windows/backup-restore-dhcp-server-database-information/

DCV1 is Server 2012 Standard Not R2

DCV1A is Server 2012r2 Standard

Targeting DCV1A as the server to migrate to

|DHCPCred|XX9iQeBcEPzydEHxyuDydoMy|

Migration is now complete, I had to change the password for the DHCPCred account as well as add dcv1a to the list of systems the service account can logonto.

I have confirmed that DCV1A is serving leases and everything seems to be working normaly.

DCV1A has traded ip addresses with DCV2 and now holds the ip address 172.20.1.102, DCV2 now holds the ip address 172.20.1.66.

To Marty:
Yesterday afternoon I migrated DHCP services to DCV1A, I had to change the password for the DHCPCred account as well as add dcv1a to the list of systems the service account can log onto.

|DHCPCred	| XX9iQeBcEPzydEHxyuDydoMy |

I have confirmed that DCV1A is serving/renewing leases and everything seems to be functioning normally.
DCV1A has traded IP addresses with DCV2 during this migration to preserve the function of any devices that had its IP address manually entered and now holds the IP address 172.20.1.102, DCV2 now holds the IP address 172.20.1.66.