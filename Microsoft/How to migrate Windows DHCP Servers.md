# How to migrate Windows DHCP Servers
#sysadmin #windows #network 

Notes taken from: https://searchwindowsserver.techtarget.com/tip/Moving-DHCP-database-to-a-new-server

Moving DHCP database to a new server

Follow the steps in this tip to configure a new DHCP server and move the current DHCP database from the old server to the new server.

Periodically system administrators may need to upgrade or decommission the DHCP server. They can use the backup and restore procedure to move the DHCP database to a new server during such situations.

Follow these steps to configure a new DHCP server and move the current DHCP database from the old server to the new server.

Start by installing the DHCP Server service on the destination server and then restart the server.

When the server restarts, log on, and at the command prompt type net stop "dhcp server" to stop the DHCP Server service.

Remove the contents of the %SystemRoot%\System32\Dhcp folder on this server.

Now log on to the original (source) server, and at the command prompt type net stop "dhcp server" to stop the DHCP Server service.

In the Services node of Computer Management, disable the DHCP Server service so that it can no longer be started, then copy the entire contents of the %SystemRoot%\System32\Dhcp folder to the %SystemRoot% System32Dhcp folder on the destination server.

Once all the necessary files are on the destination server, type net start "dhcp server" to start the DHCP Server service on the destination server, which completes the migration.
```
net start "dhcp server"
```

!!! Austins Additional Notes
When migrating DHCP check dcdiag afterwards to make sure that you dont have any errors, if you dont have one you should create a service account that can login to your domain controllers as a standard user to update Dynamic DNS entries.

Once you have created a service user for DHCP dynamic dns updates they need to be added to the group "DnsUpdateProxy", once this is done go to the snapin/managment panel for DHCP -> your new server -> properties -> advanced and enter the login info for your service account.  If you get an error message its likely that your service account does not have your DHCP server selected as a server it can login to.

To confirm that you have set the creditenals for the service account you can run:
```
netsh dhcp server show dnscredentials
```

This should show you what account you are using for updating Dynamic DNS entries.  To set the DHCP information:
```
netsh dhcp server set dnscredentials UserID domain Password
```



A failback might be to add your domain controller to the "DnsUpdateProxy" group which can function the same way but thats hacky and its much better to use a service account as microsoft specifies.

## What If?
I’m restoring from a backup of a DHCP HA Setup and the 2 old DHCP servers don’t exist anymore?
Force remove the old configuration:
```powershell
Get-DhcpServerv4Failover
Remove-DhcpServerv4Failover -ComputerName “dhcpserver.contoso.com” -Name “SFO-SIN-Failover”
```
