# Active Directory Best Practices
#sysadmin #activedirectory #notes 

[https://www.youtube.com/watch?v=_Q-rLcBKJaw](https://www.youtube.com/watch?v=_Q-rLcBKJaw)

Dan Holme presenting

* Keep domain controllers clean, don’t have them doing other things.
* Use read only domain controllers for physical locations that are less secure.
	* You must be sure you test these in production before you can expect them to work, some problems with third party applications.
* Forest and domain design
	* Active directory federation replaces domain trusts and child domains
	* OU Design
		* Design to manage security first, not group policy.
		* Delegation and assigning of permissions should be as simple as possible.
		* WMI group policy filtering is effective but performance intensive.
		* If you use groups in your filters for group policy you should also go and add authenticated users to have read permissions in the delegation tab so that other administrators can read the policy.
		* Pro tip, every time you create a group policy object create a group that is denied it even if nobody needs it yet, it will make it easier to troubleshoot later.
* Sites
	* Are logical objects that represent high connectivity
	* Not necessarily geographic locations
	* Sites are used for locating resources that are near by, this means that having one site is probably a bad idea when you have things like DFSN.
	* Sites don’t need domain controllers, you can implement coverage which tells a site without a DC to use the DC from another site.
	* Sites are defined by the IP addresses that they have.
	* DCs on the same sites use intra site replication, happens within 30seconds or so.
	* DCs on different sites take about 15min, if you have good connectivity you can enable notification based replication to get faster sync speeds.
* Administrative Roles
	* Have mutable accounts,
		* One as a user, different one for privlages
		* Use runas
	* If you are a domain admin have a 3rd account that is only that.
	* You should never logon as the Administrator account, many people lock this down so that 2 people each have part of the password too logon.
	* Powerful groups, Schema, enterprise, should be kept as empty as possible.
		* Schema can be completely empty until you need to make a change
	* Operators: these accounts are problematic as they were added to port back compatibility, they are overprivlaged, these should be empty, google adminSDHolder.
		* Create your own groups to fill these functions so you don’t have to deal with legacy garbage.
		* Audit changes to these groups
		* Active directory service change auditing.
* User Accounts
	* Create unique to domain and unique to user
		* You can let users pick their own names
	* User priceable name to logon, it can be their your email address
	* Renaming the Administrator account does not add any security, but its worth renaming to prevent techs from accidentally logging in.
* One of the biggest risks for user passwords is that they have to be complex enough to write down or that users will create ones that are not actually complex so that they can remember. Encourage users to use phrases.
	* Use fine grained password policies to increase strictness.
* Bad Practice:
	* Lastname first-name with comma, AD separates things with commas.
		* Customise MMC view instead, you can add the last name column and sort by that. Or use display name field, the common name field is a no no.
* Extreme MMC Consoling!
	* Make your own console!
	* Link to web address snapin
		* Can be used to add URL’s for a webpage.
		* Intranite sites in MMC
		* You can add a UNC Path! You can add a path to a share which exposes the share to the MMC console which will add it as an admin! Very Cool! This allows you to admin a explorer shell as an admin, super cool.
	* Saved Queries.
		* Allow you to create a view based on a criteria, regardless of OU.
		* Easy to create reports,
			* Users that have not logged on in 90 days.
		* Easy to create views that show subsets of accounts
		* Collums that are visible are per saved query
	* Taskpads
		* Bring commands that you need into the tools that you use.
		* As admins we want to RDP into a machine.
		* MSTSC command allows you to connect to a computer using a variable.
		* New tasked view, new task and shell command.
			* Shell command allows you to create a command with a variable.
			* Name it
			* Give it an icon
			* When your done you can pick a computer and run the command, the variable will be the name of the computer.
		* You can make a remote command prompt using psexec